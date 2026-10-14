
# Python, Django 설치
1. python 설치
	 - linux
		 - suto apt update
		 - sudo apt install python3
	- macos
		- https://www.python.org 에서 다운로드
2. 가상 환경 생성
	- pip로 virtualenv 패키지 설치
		- pip3 install virtualenv
	- 프로젝트 생성할 디렉토리로 이동
		- cd backend
	- 새로운 가상 환경 생성
		- python3 -m virtualenv myenv
	- 가상 환경 활성화
		- source myenv/bin/activate
3. Django설치
	- pip install django (가상환경 활성화 이후 pip3 대신 pip 사용 가능)
	- Django 설치 확인
		- django-admin --version
4. Django 프로젝트 생성
	- 프로젝트 가상 환경 수행(프로젝트 수행하기전에 항상?)
		- python3 -m virtualenv myenv 
		- 가상 환경에서 실행? : 위 명령어 대신? source myenv/bin/activate
	- django 프로젝트 생성할 디렉토리로 이동
	- 프로젝트 생성
		- django-admin startproject example_project
			- backend 디렉토리에 생성되고 테스트 서버 실행용 manage.py가 생성됨
			- 디렉토리 안에 backend 디렉토리가 하나 더 생성되어 있음
		- 생성한 디렉토리로 이동하여 개발 서버 실행
			- cd backend/chapter2/exmaple_project
			- python manage.py runserver
		- http://127.0.0.1:8000 접속하여 개발 서버 실행 확인
	- 앱 프로젝트 생성
		- 프로젝트 내에서 example_app 새로운 django 앱 생성
			- 프로젝트 위치는 backend/chapter2/example_project 
			- python manage.py startapp example_app
		- example_app/views.py 파일 수정
		```python
		from django.http import HttpResponse
		
		def hello(requet):
		    return HttpResponse("Hello, World!")
		```
		- backend/urls.py 파일 편집
			```python
			from example_app.views import hello
			
			urlpatterns = [
			    #... 기존코드
			    path('hello/', hello),
			]
			```
		- 개발 서버 실행 후 url /hello 접속
			- python manage.py runserver
1. Django Rest Framework Django프로젝트에 추가
	- Django Rest Framework 설치(위치 backend/chapter2/example_project)
		- pip install djangorestframework
	- 기존 생성한 backend/chapter2/example_project/settings.py 편집
      ```python
      INSTALLED_APPS =[
          # ...
          'rest_framework',
          # ...
      ]
      ```
    - 필요시 settings.py 에 인증, 권한, 페이지네이션 설정 등을 추가 가능
		```python
		INSTALLED_APPS = [
		    # ...
		    'DEFAULT_AUTHENTICATION_CLASSES': [
		        'rest_framework.authentication.SessionAuthentication',
		        'rest_framework.authentication.BasicAuthentication'
		    ],
		    'DEFAULT_PERMISSION_CLASSES': [
		        'rest_framework.permissions.IsAuthenticated',
		    ]
		]
		```
2. REST API 생성
	- example_app/views.py 편집
		```python
		from rest_framework.decorators import api_view
		from rest_framework.response from Response
		
		@api_view(['GET'])
		def hello_rest_api(request):
		    data = {'message': 'Hello, REST API!'}
		    return Response(data)
		```
1. urls.py  편집
    ```python
    from example_app.views import hello_rest_api
    
    ...
    urlpatterns = [
    path('api/hello', hello_rest_api, name='hello_rest_api'),
    ]
    ```
1. 개발 서버에서 테스트(python manage.py runserver)
	- 403 오류 발생
		- API view에 대한 인증 필요
2. example_app/views.py 에 인증 추가
	 ```python
	 from rest_framework.decorators import api_view, permission_classes
	 from rest_framework.permissions import AllowAny
	 ...
	 
	 @api_view(['GET', 'POST'])
	 @permission_classes([AllowAny])
	 def hello_rest_api(request):
	     data = {'message': 'Hello, REST API!'}
	     return Response(data)
	 ```
		
1. 개발 서버에서 테스트

- Django REST Framework 주요 구성 요소
	- Serializers
		- Django 모델과 같은 복잡한 데이터 유형을 JSON, XML 또는 기타 콘텐츠 유형으로 변환하여 응답으로 렌더링하거나 클라이언트가 이해할 수 있는 형식으로 데이터를 파싱하는 기능 제공
			```python
			from rest_framework import serializers
			
			class BookSerializer(serializers.Serializer):
			    title = serializers.CharField(max_langth=100)
			    author = serializers.CharField(max_length=100)
			    publication_data = serializers.DateField()
			    
			    def create(self, validated_data):
			        return Book.objects.create(**validated_data)
			```
	- Views
		- HTTP 요청을 처리하고 HTTP 응답을 반환하는 역할
		- 뷰는 종종 Serializers를 사용하여 클라이언트가 이해할 수 있는 형식으로 데이터를 반환
		- APIView, GenericAPIView, ViewSet과 같은 내장 뷰를 제공
		- 예제
			```python
			from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class BookListView(APIView):
    def get(self, request):
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = BookSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

			```
	- Routers
		- API 뷰에 대한 URL패턴을 자동으로 생성. 
		- 라우터는 API 엔드포인트를 조직하는 간단하고 일관된 방법을 제공, API 버전을 관리하거나 네임스페이스 접두사를 적용하기 쉽게 함
		- 예제
		 ```python
		 from rest_framework import routers
from .views import BookViewSet

router = routers.DefaultRouter()
router.register(r'books', BookViewSet)

		 ```
	- Authentication
		- 토큰 기반 인증, 세션 인증, OAuth2와 같은 다양한 인증 메커니즘 지원
		- 예제
          ```python
          from rest_framework.authentication import TokenAuthentication
from rest_framework.permissions import IsAuthenticated

class BookListView(APIView):
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]

    # 뷰 코드...

          ```
	- Permissions
		- API 리소스에서 특정 작업을 수행할 수 있는 사용자를 제어
		- 예제
         ```python
         from rest_framework.permissions import IsAdminUser

class BookListView(APIView):
    permission_classes = [IsAdminUser]

    # 뷰 코드...

         ```
	- Throttling
		- API를 남용하거나 과도하게 사용하는 것으로부터 보호
		- 클라이언트가 특정 시간 프레임 내에서 요청할 수 있는 횟수를 제어
		- 여러 내장 쓰로틀링 클래스를 제공하여 사용자 정의 쓰로틀링 클래스를 만들어 요구 사항에 맞게 설정 가능
		- 예제
			```python
			from rest_framework.throttling import UserRateThrottle

class BookListView(APIView):
    throttle_classes = [UserRateThrottle]

    # 뷰 코드...

			```
	- Pagination
		- 대량의 데이터 집합을 관리하는 API의 성능을 향상
		- limit/offset 페이지네이션, 커서 페이지네이션 등 다양한 페이지네이션 스타일 지원
		- 에제
			```python
			from rest_framework.pagination import PageNumberPagination

class CustomPagination(PageNumberPagination):
    page_size = 10
    page_size_query_param = 'page_size'
    max_page_size = 100

class BookListView(APIView):
    pagination_class = CustomPagination

    # 뷰 코드...

			```
	- Filtering
		- API에서 반환되는 데이터를 검색, 정렬, 필터링하는데 도움
		- 예제
			```python
			from django_filters.rest_framework import DjangoFilterBackend

class BookListView(APIView):
    filter_backends = [DjangoFilterBackend]
    filterset_fields = ['title', 'author']

    # 뷰 코드...

			```
	- Exceptions
		- 유효성 검사 오류, 인증 실패, 권한 거부 등과 같은 일반적인 API 오류를 처리하는 내장 예외 클래스 포함
		- 예제
			```python
			from rest_framework.views import exception_handler

def custom_exception_handler(exc, context):
    response = exception_handler(exc, context)
    if response is not None:
        custom_data = {'detail': response.data['detail']}
        response.data = custom_data
    return response

			```
	- Documentation
		- OPENAPI 표준 지원 및 브라우저에서 작동하는 API문서 생성 및 유지를 위한 여러 도구 제공
		- 예제
			```python
			from rest_framework import routers
from drf_yasg import openapi
from drf_yasg.views import get_schema_view

schema_view = get_schema_view(
    openapi.Info(
        title="Book API",
        default_version='v1',
        description="API for managing books",
        terms_of_service="https://www.example.com/terms/",
        contact=openapi.Contact(email="contact@example.com"),
        license=openapi.License(name="BSD License"),
    ),
    public=True

,
    permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
    # 기존 URL 패턴들...
    path('api/docs/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    path('api/redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
]

			```

- 사전에 필요한 패키지들은 requirements.txt 에 작성해 놓았다가 pip 명령으로 설치
	- 사전에 requirements.txt 파일을 수동으로 작성하나?
	- 명령어: pip install -r requirements.txt
# Node.js, React, Next.js 설치
- Node, npm설치
- creat react app를 전역으로 설치
	- npm install -g create-react-app
- react 프로젝트 생성
	- cd cerberus/frontend/chapter2
	- create-react-app my-react-app
	- cd my-react-app
	- npm start (개발 서버 실행)
- recat 프로젝트에 next.js 설치(next.js와 필요한 종속성 설치)
	- cd my-react-app
	- npm install --save next react react-dom
- package.json파일  편집(next.js 개발 서버 실행, 빌드 등)
	- 예제
		```json
		"scripts":{
		  ...
		},
		"scripts": {
		  "dev": "next dev",
		  "build": "next build",
		  "start": "next start",
		  "export": "next export"
		},
		```
- next.js 모든 파일 저장 디렉토리 생성
	- mkdir pages (frontend/chapter2/my-react-app/src/pages)
- 간단한 index.js 생성
	```js
	import React from 'react';

const IndexPage = () => {
  return (
    <div>
      <h1>Welcome to Next.js!</h1>
    </div>
  );
};

export default IndexPage;

	```
- 개발 서버 실행
	- npm run dev

- 기타 작업 도구
	- API테스트 
		- postman
		- insomnia
	- 작업 실행 및 빌드도구
		- webpack: 자바스크립트 모듈 번들러 및 빌드 도구, 어플리케이션 성능 향상
		- gulp: 워크 플로우를 자동화
		- grunt: 개발 워크 플로우 자동화
	- 린터 밑 코드 포매터(코드 스타일 유지)
		- prettier
		- eslint

# Django framework
## MVT아키텍처
- Model : 데이터와 비즈니스 로직 처리
- View : 비즈니스 로직 시행 및 모델 상호 작용
- Template: 데이터의 표시 및 렌더링
- 역할 : 데이터 처리와 표시에 특화
- 관계: 뷰는 모델 및 템플릿과 상호 작용

## Django 구조
- Django프로젝트: 전체 웹 애플리케이션
- Django앱: 프로젝트 내에서 특정한 기능을 제공하는 모듈
- 프로젝트는 여러 개의 앱을 포함할 수 있으며 각 앱은 다양한 프로젝트에 연결될 수 있음
- Django 프로젝트 자동 생성된 파일과 디렉토리
	- manage.py: 관리 작업을 위한 명령 줄 유틸리티
	- settings.py: 전체 Django프로젝트의 구성
	- urls.py: URL을 기반으로 웹 요청을 라우팅
	- wsgi.py, asgi.py: WSGA-나 ASGI-호환 웹 서버가 프로젝트르 제공하는데 사용
- Djago 앱 파일
	- models.py: 데이터베이스 스키마
	- views.py: 파일, 비즈니스 로직 담당
	- tests.py: 단위 테스트 담당
	- urls.py: 앱별 URL라우팅을 위한 파일
	- admin.py: 관리자 인터페이스를 구성
	- forms.py: Django 폼을 정의하는 파일

## 프로젝트 생성
- 새로운 Django 프로젝트 생성
	- django-admin startproject your_project_name
- 새로운 Django앱 생성
	- Django프로젝트의 루트 디렉토리(manage.py 위치) 에서 생성(웹 프로젝트도 루트 디렉토리 하위에 존재)
	- python manage.py startapp your_app_name

## Django Model
- 데이터에 대한 단일하고 정확한 정보
- 저장하는 데이터의 필수 필드와 동작 포함
- DRY(Don't Repeat Yourself)원칙을 따라 데이터 모델을 한 곳에서 정의하고 자동으로 유도하는 것을 권장
- Model은 django.db.models.Model 클래스를 상속
- DRY 원칙
	- 중복을 제거
	- 추상화와 재사용
	- 단일 진리원칙: 데이터 또는 비즈니스 규칙과 같은 중요한 정보는 하나의 소스에서 관리
### 모델과 필드 정의
- 모델 정의
```python
from django.db import models

class BlogPost(models.Model):
	title = models.CharField(max_length=200)
	content = models.TextField()
	published_date=models.DateTimeField(auto_now_add=True)
```

- Django Model 데이터 유형
 | 데이터 유형 | 설명 | 비고 |
 | --- | --- | --- |
 |CharField | 문자열, 텍스트 데이터 저장 | max_length로 최대 길이 지정|
 |IntegerField| 정수값, 양수와 음수  모두 특정 범위 내에서 저장| |
 |FloatField| 소수점 정수, 지정된 소수 자릿수를 가진 부동 수수점 숫자 저장||
 |BooleanField|Boolean값 저장, 이진 값이나 플래스 저장||
 |DateField|날짜 저장|"YYYY-MM-DD"형식|
 |DateTimeField|날짜와 시간값 표시|YYYY-MM-DD HH:MM:SS 형식 표시|
 |EmailField|이메일 주소|입력된 값이 유효한 이메일 주소인지 검증|
 |FileField:파일 업로드 표시. 파일 경로나 실제 파일 객체 저장||
 |ForeignKey|두 모델간의 다대일 관계 정의에 사용.||
 |ManytoManyField|두 모델간의 다대다관계 정의 사용||

#### 모델 간의 관계 정의
- 일대일(User - UserProfile)
```python
from django.db import models
from django.contrib.auth.models import User

class UserProfile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    bio = models.TextField()
```

- 일대다(BlogPost:Comment)
```python
class Comment(models.Model):
    post = models.ForeignKey(BlogPost, on_delete=models.CASCADE)
    text = models.TextField()
```

- 다대다
```python
class Tag(models.Model):
    name = models.CharField(max_length=30)
    posts = moels.ManyToManyField(BlogPost)
```

#### 모델 마이그레이션과 데이터 베이스 스키마 관리
- 모델 마이그레이션은 모델을 변경할 때 데이터 베이스 스키마에 전파하는 Django의 방법
- 명령어
	- 모델 변경 후, 마이그레이션 생성
		- python manage.py makemigrations
	- 마이그레이션을 데이터베이스에 적용
		- python manage.py migrate
- Django의 ORM기능을 기반으로 작동

#### Django ORM 
- 간단한 처리
```python
# 모델의 모든 인스턴스 검색
instances = YourModel.objects.all()

# 필터링된 인스턴스 검색
filtered_instances = YourModel.objects.filter(your_field='your_value')

# 단일 인스턴스 검색
instance = YourModel.objects.get(id='your_id')
```

- 여러 필터 연결 및 기타 처리
```python
# 여러 개의 필터 조건 연결
filtered_instances = YourModel.objects.filter(your_field1='value1', your_field2='value2')

# 대소문자 구분 없는 포함 여부 확인
filtered_instances = YourModel.objects.filter(your_field__icontains='value')

# 관계를 걸처 조회
filtered_instances = Entry.objects.filter(blod__name='Django')
```

- 모델의 새 인스턴스 생성: Manager.create() 사용
- 인스턴스 업데이트 .save() 사용
``` python
# 새 인스턴스 생성
net_instance = YourModel.objects.create(your_field='your_value')

# 인스턴스 업데이트
instance.your_field = 'new_value'
instance.save()
```

- annotate(), aggregate() : 집계 및 복잡한 쿼리 지원
- Django쿼리틑 lazy방식으로 동작. 쿼리가 필요한 경우에만 동작
- 데이터베이스 기본 쿼리 명령어
	- save() : 인스턴스 DB 저장
	- filter(): 특정 조건 일치 객체 검색
	- get(): 특정 조건 기반 단일 객체 조회
	- exclude(): 특정 조건과 일치하지 않는 객체 제외
	- order_by() : 특정 필드 기준 정렬
	- 슬라이싱([start:stop]): 검색된 객체의 수를 제한
	- delete() : 데이터 베이스에서 삭제

- 샘플
```python
# models.py
from django.db import models

class Author(models.Model):
  name = models.CharField(max_length=100)
  age = models.IntegerField()
  
  def __str__(self):
      return self.name
      
class Book(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey(author, on_delete_models.CASCADE)
    
    def __str__(self)
        return self.title
```

```python
# views.py
from django.shortcuts import render
from .models import AUthor, Book

def home(request):
    # 객체 생성 및 데이터베이스에 저장
    author = Author(name='John Doe', age=30)
    author.save()
    
    book = Book(title='My Book', author=author)
    book.save()
    
    # 데이터베이스에서 객체 검색
    books = Book.objects.all(). # 모든 책 검색
	    book = Book.objects.get(title='My Book')  # 특정 책 검색
	    
	# 객체 업데이트
	book.title = 'New Book Title'
	book.save()
	
	# 객체 삭제
	book.delete()
	
	return render(request, 'home.html')
```

#### Django ORM의 쿼리 API
- 필터링
	- filter(\*\*kwargs): 주어진 조회 매개변수와 일치하는 Queryset 반환
	- exclude(\*\*kwargs):  주어진 조히 매개변수와 일치하지 않는 QuerySet 반환
	- get(\*\*kwargs): 주어진 조회 매개변수와 일치하는 단일 객체 반환, 일치하는 항목이 없을 경우 DoesNotExist 예외 발생. 여러 개의 일치 항목이 있으면 MultipleObjectsReturned 예외 발생
	- first(): QuerySet() 에서 첫번째 객체 반환
	- last(): QeurySet()에서 마지막 객체 반환
- 쿼리 연결
	- filter().exclude(): 여러 개의 필터링 조건을 연결
	- filter().order_by(): 결과의 정렬 순서를 지정
	- filter().values(): 모델 인스턴스 대신 딕셔너리를 포함하는 QuerySet 반환
- 집계
	- aggregate(\*\*kwargs): QuerysSet에서 집계 작업 수행
- 어노테이션
	- annotate(\*\*kwargs): 제공된 어노테이션을 기반으로 QuerySet의 각 객체에 필드를 추가
- 관련 객체
	- select_related(): 조인을 사용하여 관련된 객체를 단일 데이터베이스 쿼리로 가져옴
	- prefetch_related(): 관련된 객체를 효율적으로 가져오기 위해 별도의 쿼리에서 미리 가져옴
- 조인과 관계
	- join(): 테이블 간에 명시적인 조인 수행
	- select_related(): 조인을 사용하여 관련된 객체를 가져와 데이터베이스 쿼리 수를 줄임
	- prefetch_related(): 별도의 쿼리를 사용하여 관련된 객체를 효율적으로 가져옴
	- reverse(): 역방향 관계를 따라 관련된 객체를 가져옴
- 정렬과 정렬 순서
	- order_by(\*fields): 지정된 필드를 기준으로 QuerySet을 정렬
	- reverse(): QuerySet의 순서를 반전
- 제한과 슬라이싱
	- all(): QuerySet에 있는 모든 객체 반환
	- values_list(\*fields): 모델 인스턴스 대신 튜플을 포함하는 QuerySet을 반환
	- values(\*fields): 모델 인스턴스 대신 딕셔너리를 포함하는 QuerySet을 반환
	- distinct(): 고유한 값을 가진 QuerySet  반환

## Django View
- 웹 요청을 받아 처리하고 웹 응답을 반환하는 Python  함수
- 애플리케이션의 데이터와 사용자 인터페이스를 연결하는 역할. 로직을 처리

### View 작성 방법
- 함수 기반 뷰(Function-Based Views, FBVs)
- 클래스 기반 뷰(Class-Based Views. CBVs)
- 차이점
||함수 기반 뷰| 클래스 기반 뷰|
| --- | --- | --- |
|장점| 간단하고 이해하기 쉽다| 코드 재사용성을 통한 개발 속도 향상|
| | 유연하며 복잡한 로직과 커스터 마이징 가능| 내장된 기능을 통한 간편한 구현|
||적은 보일러플레이트 코드를 가짐| 관심사를 분리하여 코드 유지보수성 향상|
||익숙한 python함수로 작성되어 진입 장벽이 낮음||
|단점|반복적인 코드 작성으로 중복 코드 발생 가능| 객체 지향 기능의 제한으로 일부 복잡한 상황에서 코드 이해 어려움|
||코드 재사용성 부족, 유지보수 여러움||러닝 커브가 높음|
||객체 지행 프로그래밍의 이점 활용 불가|구문이 상대적으로 장황하고 코드가 길어짐|

- 예제(함수 기반 뷰)
```python
from django.http import HttpResponse

def hello_world(request):
    return HttpResponse("Hello, World!")
```

- 예제(클래스기반 뷰)
```python
from django.views import View
from django.http import HttpResponse

class HelloWorldView(View):
    def get(self, request):
        return HttpResponse("Hello, World!")
```

- View 작성 규칙
	- View는 최소 하나의 매개변수를 가져야 함(request)
	- View는 HttpResponse또는 그 하위의 객체를 반환
	- URLconf  또는 URL구성을 사용하여 라우팅
		```python
		# URLconf
		from django.urls import Path
		from .views import HelloWorldView
		
		urlpatterns = {
		    path('hello/;, HelloWorldView.as_view()),
		}
		```
	- URL 패턴을 URLconfs라 불리는 python 모듈에 정의
	- URLConf 는 path함수나 복잡하거나 기존의 URL패턴에 대해서는 re_path함수를 통해 뷰 함수와 매핑
- Django의 내장 제너릭 뷰 사용
	- 일반적인 웹 개발 작업을 처리하기 위한 몇가지 제너릭 클래스 기반 뷰
	- django.view.generic.View를 상속
		- TemplateView: 주어진 템플릿을 렌더링
		- RedirectView: 다른 URL로 리디렉션을 제공
		- ListView: 객체 목록을 표시
		- DetailView: 단일 객체의 상세 뷰 표시
		- CreatedView,UpdateView, DeleteView: 객체를 생성, 업데이트, 삭제하기 위한 폼 제공
	- 예제
		```python
		from django.views.generic import ListView
		from .models import Book
		
		class BookListView(ListView):
		    model = Book
		```

### Django REST 프레임워크에서의 Django View
- view는 API엔드 포인트를 정의하고 데이터를 검색, 생성, 삭제하는 방법을 정의하는데 사용?
- 함수기반 뷰
	```python
	from rest_framework.decorators import api_view
	from rest_framework.response import Response
	
	@api_view(['GET'])
	def hello_world(request):
	    data = {'message': 'Hello, World!'}
	    return Response(data)
	```

- 클래스 기반 뷰
```python
from rest_framework.views import APIView
from rest_framework.response import Response

class HelloWorldView(APIView):
    def get(self, request):
        data = {'message': 'Hello, World!'}
        return Response(data)
```

- 일반 view와 다른 것은 import하는 객체가 다른 것, 반환할 때 HttpResponse가 아닌 rest_framework에서 제공하는 Response 사용
- URL 도 urls.py파일에 URL패턴을 정의하여 처리

## Django 템플릿
- 뷰에서 제공하는 컨텍스트에 따라 동적인 HTML페이지 생성 : 템플릿 시스템 사용 
- 템플릿은 기본적으로 텍스트 파일. 텍스트 기반 형식(HTML, XML, CSV)을 생성 가능
- Django프로젝트에서 각 Django앱은 기본적으로 templates 디렉토리 생성됨
- 템플릿 상속
	- 사이트의 모든 공통 요소를 포함하는 기본 "스켈레톤" 템플릿
	- 자식 템플릿이 오버라이드할 수 있는 블록 정의 가능
- 템플릿 태그: 템플릿에서 논리 제공
	- {%, %} 로 둘러싸임.
	- {% for %} 반복, {% if %} 조건문, 오버라이드 블록 정의 {% block %} 등 사용
- 뷰에서 템플릿으로 데이터를 렌더링하기 위해 변수 사용
- 템플릿에 컨텍스트 전달 할 때(일반적으로 딕셔너리 형태), 
- 내장 템플릿 필터와 태그 사용
	- date : 날짜와 시간 객체 서식화 
	- ex : {{ book.publication_date|date:"F j, Y"}} "2023년 1월 1일" 같은 형식으로 출력

### Django 템플릿 예시
- 템플릿 생성과 구성
	- 각 앱의 templates 디렉토리에 템플릿 저장

#### 템플릿 상속과 템플릿 태그
- 기본 템플릿
```html
<!-- base.html -->
<html>
 <head>
   <title>{% block title %}My Website{% endblock %}</title>
 </head>
 <body>
   <header>
     <h1>My Website</h1>
   </header>
   <nav>
     <ul>
       <li><a href="/">Home</a></li>
       <li><a href="/about/">About</a></li>
       <li><a href="/contact/">Contact</a></li>
     </ul>
   </nav>

   {% block content %}
   {% endblock %}

   <footer>
     &copy; 2023 My Website. All rights reserved.
   </footer>
 </body>
</html>

```

- 자식 템플릿
```html
<!-- home.html -->
{% extends 'myapp/base.html' %}

{% block title %}Home{% endblock %}

{% block content %}
<h2>Welcome to my website!</h2>
<p>This is the home page.</p>
{% endblock %}

```
- {% extends %} 태그로 base.html 템플릿 상속
- {% block %} 자식 템플릿에서 오버라이드할 수 있는 템플릿 섹션 정의

#### 뷰에서 템플릿 데이터 전달
- 뷰
```python
from django.shortcuts import render

def home(request):
    data = {
        'name': 'John Doe',
        'age': 25,
        'country': 'USE'
    }
    return render(request, 'myapp/home.html', context=data)
```

- 템플릿
```html
<h2>Welcome, {{ name }}!</h2>
<p>Your age is {{ age }} and your are from {{ country }}. </p>
```

#### Django의 내장 템플릿 필터와 태그 사용
- 템플릿
```html
<!-- home.html -->
<h2>Welcome, {{ name|capfirst }}!</h2>
<p>Your age is {{ age }} and you are from {{ country|upper }}.</p>

```
- capfirst 필터를 사용하여 이름의 첫글자를 대문자로, upper로 국가 이름은 대문자로 변환