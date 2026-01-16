1.django-admin startproject project
2.python manage.py startapp app
3.pip install djangorestframework_simplejwt
4.in settings.py
	INSTALLED_APPS = [
   	 	'django.contrib.admin',
    		'django.contrib.auth',
    		'django.contrib.contenttypes',
    		'django.contrib.sessions',
    		'django.contrib.messages',
    		'django.contrib.staticfiles',
    		'rest_framework',
    		'app',  
			]


5.	INSTALLED_APPS = [
    		'django.contrib.admin',
    		'django.contrib.auth',
    		'django.contrib.contenttypes',
    		'django.contrib.sessions',
    		'django.contrib.messages',
    		'django.contrib.staticfiles',
    		'rest_framework',
    		'app',  
			]
	REST_FRAMEWORK = {
    		'DEFAULT_AUTHENTICATION_CLASSES': [
        		'rest_framework_simplejwt.authentication.JWTAuthentication',
    						  ],
			  }


6. then in main urls:
	from django.urls import path, include
	from rest_framework_simplejwt import views as jwt_views

	urlpatterns = [
    		path('api/token/', jwt_views.TokenObtainPairView.as_view(), name ='token_obtain_pair'),
    		path('api/token/refresh/', jwt_views.TokenRefreshView.as_view(), name ='token_refresh'),
    		path('', include('app.urls')),
			]
7. then in app.urls
	from django.urls import path
	from . import views

	urlpatterns = [
    		path('hello/', views.HelloView.as_view(), name ='hello'),
			]

8.  then in app.views
	from rest_framework.views import APIView
	from rest_framework.response import Response
	from rest_framework.permissions import IsAuthenticated


	class HelloView(APIView):
    		permission_classes = (IsAuthenticated, )

    		def get(self, request):
        		content = {'message': 'Hello, GeeksforGeeks'}
        		return Response(content)

9. 	python manage.py migrate
10.	python manage.py createsuperuser
11.	python manage.py runserver

12. in post man
	->http://127.0.0.1:8000/api/token/
	->in body->raw
		{
    		"username":"salman",
   		 "password":"salman"
		}
		#the username and password both are true
	->method=post and send

	->in down ->json -> you will get 
		{
    		"refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTc2ODEyNjIxOCwiaWF0IjoxNzY4MDM5ODE4LCJqdGkiOiJjM2Y5NjVmMzE5MjU0MjNmYTYyZjUxNDRkNDRjYjVmMSIsInVzZXJfaWQiOiIxIn0.4Ey65M_-iUvETAT3BPwfK9vLWQyKmfOpEMBPOgUlpUA",
    		"access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzY4MDQwMTE4LCJpYXQiOjE3NjgwMzk4MjAsImp0aSI6IjU5NmY4YTA1YzFlZDRmNWNiZjc2MDNlZjA2Mzc4MjhiIiwidXNlcl9pZCI6IjEifQ.D6HchZU8-2qYUySoyd6f4lzm7AWLvxaOuaQBe6OjjtM"
		}

13. 	put this access token in autherization->Barear Token-> input box with yellow border
		
14. then enter http://127.0.0.1:8000/hello/  with get request
	if your token is not expired or true you will get 
	{
   		 "message": "Hello, GeeksforGeeks"
	}
	
	if we give enter in the url http://127.0.0.1:8000/hello/ without token 
		{
    			"detail": "Authentication credentials were not provided."
		}
			this message response , which is jwt modules itself response in DRF
	
	if token is invalid or wrong then this response will come 
			{
  		"detail": "Given token not valid for any token type"
			}
	if token will expire then this response will come 
			{
    "detail": "Given token not valid for any token type",
    "code": "token_not_valid",
    "messages": [
        {
            "token_class": "AccessToken",
            "token_type": "access",
            "message": "Token is expired"
        }
    ]
}


15. 	if access token is expired then 
	enter this url in postman http://127.0.0.1:8000/api/token/refresh/ 	with post method
	enter this in body-> raw
	{
   		"refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTc2ODEzMjI2MSwiaWF0IjoxNzY4MDQ1ODYxLCJqdGkiOiJiMzc5NGNjYzdmMTU0MWViOGY5MDJiNzg2Y2RhOTU5YiIsInVzZXJfaWQiOiIxIn0.1sQcMIbJOKNrWkoZHk02wAtJupurxh3FALmdjknBAp4"
	}
	this is your refresh token
	then you will get new access token


