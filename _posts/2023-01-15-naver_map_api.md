---
published: False
title:  "[네이버 지도 api] 행정 경계 표시하기"
categories: Etc
tag: [naver-map-api, 네이버지도]
---

네이버 지도 api 행정 경계 표시하기  

네이버 지도를 사용하는 방법을 사용하는 방법은  
문서도 잘 나와있고 블로그 글도 많기에 생략한다.  


먼저 행정구역 경계 데이터가 필요한데  
vworld라는 곳에서 api로 원하는 행정구역의 경계 데이터를 얻을 수 있다.

[VWORLD 공간정보 플랫폼]<https://www.vworld.kr/v4po_main.do> 

링크로 들어가서 회원가입을 하고 로그인한다.
![Image0](/images/2022-11-23-NUMA_0.png)

인증키를 먼저 발급받는다.

2D 데이터 항목에서 동읍면을 검색하면 API가 나온다.  
다른 단위의 행정구역이 필요하면 필요에 맞게 검색하면 된다.  

API 사용법이 나오는데 대략 이런식이다.

파라미터로  
key : 발급받은 API 키값  
domain : 서비스되는 사이트의 주소  
attrFilter : 원하는 방식의 행정구역 양식  
행정동코드를 사용해도 되고 검색방식을 사용해도 된다.  
나의 경우는 행정동 코드를 사용했다.

포스트맨으로 해당 api에 요청해보았다.  
결과값에서 coordinates 항목이 경계를 그릴 수 있는 좌표 목록이다.  
![Image0](/images/2022-11-23-NUMA_0.png)  


## 파이썬으로 데이터 가져오기

requests.get을 사용해 요청을 보내면 json 형식으로 데이터가 온다.  
결과값에서 coordinates 항목을 가져와서 js쪽으로 넘겨주면 된다.    


```python
service_url = dbid.get_service_url()
geocode = res_code["code"]
key = dbid.get_vworld_key()
url_geo = f"http://api.vworld.kr/req/data?service=data&request=GetFeature&data=LT_C_ADEMD_INFO&key={key}&domain={service_url}&attrFilter=emdCd:=:{geocode}"

with requests.get(url_geo) as page: # 페이지 가져오기
    try:
        page.raise_for_status()
    except requests.exceptions.HTTPError as Err: # 에러처리
        print(Err)
    else:
        res_geo = json.loads(page.content) # 제이슨으로 변환
        if res_geo["response"]["status"] != "OK": # 스테이터스 OK 인지 확인
            print(res_geo["response"])
        coord = res_geo["response"]["result"]["featureCollection"]["features"][0]["geometry"]["coordinates"][0][0] # 경계좌표목록 : js쪽으로 넘겨준다.
        
```

원하는 곳의 지도를 생성하고  
polyline으로 경계를 그려주면 된다.  

```js
// 지도 생성
var mapOptions = {
    center: new naver.maps.LatLng(35.2056295, 129.078463), // 지도 중심
    mapTypeId: 'normal',
    scaleControl: true,
    logoControl: false, 
    mapDataControl: true,
    minZoom: 7, // 최소 줌
    maxZoom: 15, // 최대 줌
    zoomControl: true, // 줌 컨트롤러
    zoomControlOptions: {
        style: naver.maps.ZoomControlStyle.LARGE, // 줌 컨트롤 크기
        position: naver.maps.Position.TOP_RIGHT // 줌 컨트롤 위치
    }
};

// 맵 생성
var map = new naver.maps.Map(document.getElementById('map'), mapOptions);

searchAddressToCoordinate("{{kw}}")

// 검색한 주소의 정보를 insertAddress 함수로 넘겨준다.
function searchAddressToCoordinate(address) {
    naver.maps.Service.geocode({
        query: address
    }, function(status, response) {
        if (status === naver.maps.Service.Status.ERROR) {
            return alert('Something Wrong!');
        }
        if (response.v2.meta.totalCount === 0) {
            return alert('올바른 주소를 입력해주세요.');
        }
        var htmlAddresses = [],
            item = response.v2.addresses[0],
            point = new naver.maps.Point(item.x, item.y);
        if (item.roadAddress) {
            htmlAddresses.push('[도로명 주소] ' + item.roadAddress);
        }
        if (item.jibunAddress) {
            htmlAddresses.push('[지번 주소] ' + item.jibunAddress);
        }
        if (item.englishAddress) {
            htmlAddresses.push('[영문명 주소] ' + item.englishAddress);
        }

        // 지도 이동
        var latitude = item.x
        var longitude = item.y
        var offset = 0.035
        map.morph(new naver.maps.LatLng(longitude, latitude-offset),14)
        
        // 마커 생성
        var marker = new naver.maps.Marker({
            map: map,
            position: new naver.maps.LatLng(longitude, latitude),
        });

        // 경계선 생성
        var polyline = new naver.maps.Polyline({
            path: {{coord}},
            strokeColor: '#0000FF',
            strokeOpacity: 0.8,
            strokeWeight: 6,
            zIndex: 2,
            clickable: true,
            map: map
        });

    });
}
```