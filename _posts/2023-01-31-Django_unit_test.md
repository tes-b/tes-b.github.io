---
published: False
title:  "[Django] 장고 restframework api 테스트 사용법"
categories: Django
tag: [django, unittest, python, restframework, drf]
---


drf로 제작한 api를 자체적으로 테스트 해볼 수 있다.

기본적인 사용법은 ```APITestCase```를 상속받는 클래스를 만들고  
test_로 시작하는 메소드를 만들면 된다.


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
        """

    # 리스트 불러오기
    def test_board_list_success(self):
        print("TestBoard_list_success")
        # client = APIClient()
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