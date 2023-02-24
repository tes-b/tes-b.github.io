---
published: True
title:  "[Django] 장고 restframework api 테스트 사용법"
categories: Django
tag: [django, unittest, python, restframework, drf]
---


drf로 제작한 api를 자체적으로 테스트 해볼 수 있다.  
테스트를 사용해서 서버를 켜고 직접 기능을 테스트하지 않아도 되기 때문에 편리하며  
많은 수의 테스트를 한번의 명령어로 해 볼 수 있기에 효율이 좋다.  
또한 테스트시 임시로 db에 테스트용 테이블을 만들고 테스트 후 알아서 지워주기 때문에  
작업중인 db에 변화없이 테스트 해볼 수 있어서 상당히 깔끔하다.

## 테스트 작성  

기본적인 사용법은 장고 앱 내에 ```tests.py```에서  
테스트 코드를 작성해주면 된다.  
```APITestCase```를 상속받는 클래스를 만들고  
```test_```로 시작하는 메소드를 만들면 된다.

필요하다면 ```setUp```메소드를 오버라이딩 해서 사용할 수 있는데  
테스트에 필요한 세팅을 미리 만들어 놓으면 
각 test 메소드 동작전 실행된다.

예를들어 게시글을 지우는 테스르를 하기 위해 ```test_delete_post```라는 메소드를 만들었다면  
```setUp```메소드에서 미리 지울 게시글을 작성해 둔다.    
```test_delete_post``` 메소드가 실행되기 전 ```setUp``` 메소드가 실행되어 게시글을 작성하고  
```test_delete_post``` 메소드에서 글을 지우는 테스트를 할 수 있다.    

```python
# APITestCast를 임포트 한다.
from rest_framework.test import APITestCase 

# 임포트한 APITestCase를 상속받는 클래스를 만든다.
class TestBoard(APITestCase):
    
    def setUp(self):
        """
        테스트 전 실행될 함수
        오버라이딩 되는 함수이기 때문에 함수이름을 똑같이 써야 한다.
        setup, SetUp 등으로 쓰면 안됨

        이 메소드는 각 test 메소드 시작전에 실행된다.  
        """

    # 리스트 불러오기
    def test_board_list_success(self):
        print("TestBoard_list_success")
     
        board_url = "/board/api/list/"
        self.response = self.client.get(board_url, format='json')
        self.assertEqual(self.response.status_code, status.HTTP_200_OK) 

    # 글 작성 성공
    def test_board_create_success(self):
        print("TestBoard_create_success")
        self.refresh = RefreshToken.for_user(self.user)
        self.client.credentials(HTTP_AUTHORIZATION = f'Bearer {self.refresh.access_token}')

        data = {
            "author_id":self.user.id,
            "title":"title",
            "content":"content"
        }

        self.create_question_url = "/board/api/create/"
        self.response = self.client.post(self.create_question_url, data=data, format='json')
        self.assertEqual(self.response.status_code, status.HTTP_201_CREATED)    
```

## 테스트 실행

가장 기본적인 실행법이다.  
```
python3 manage.py test
```
이 명령은 현재 경로에서 test*.py 패턴을 만족하는 모든 파일을 찾은 후  
이들 테스트를 적합한 베이스 클래스를 이용해서 실행한다.  (라고 써있다.)  

만약 특정 앱의 테스트만 실행하고 싶다면 아래처럼 할 수 있다.
```
python3 manage.py test 앱이름.tests

// 예시 : 앱 이름이 board인 경우
python3 manage.py test board.tests
```

테스트를 실행하면 몇개의 테스트를 실행했고 얼마의 시간이 걸렸는지 보여준다.  
OK가 뜨면 문제없이 동작한다는 뜻이다.  
![Image0](/images/2023-02-24-Django_api_test_0.png)

### 참고
<https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Testing>
