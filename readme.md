# Step 1: Install Required Packages

Install Django, Django Rest Framework and Simple-JWT:

```
pip install django djangorestframework djangorestframework_simplejwt
```

# Step 2: Setting Up a Django Project

```
django-admin startproject <myproject>
cd myproject
```
Look:
```
myproject/
    ├─── myproject/
    |    ├── __init__.py
    |    ├── settings.py
    |    ├── urls.py
    |    ├── wsgi.py
    |    └── asgi.py
    └─── manage.py
```
## Step 3: Setting Up a Django App

```
    python manage.py startapp <myapp>
```
look after create app:

```
    myproject/
    ├── myproject/
    |   ├── __init__.py
    |   ├── settings.py
    |   ├── urls.py
    |   ├── wsgi.py
    |   └── asgi.py
    ├── myapp/
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── apps.py
    │   ├── migrations/
    │   │   └── __init__.py
    │   ├── models.py
    │   ├── tests.py
    │   ├── urls.py
    │   └── views.py
    └── manage.py
```
## Step 4: Configuring Django Rest Framework
project's settings.py file:
```
INSTALLED_APPS = [
    # All default apps go here

    'rest_framework',
    'rest_framework_simplejwt',

    'myapp',  # Replace 'myapp' with the name of your app
]
```


## Step 5: Configuring JWT Settings

project's settings.py file:
```
# settings.py
from datetime import timedelta # import this library top of the settings.py file

# put on your settings.py file below INSTALLED_APPS
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=60),
    'SLIDING_TOKEN_REFRESH_LIFETIME': timedelta(days=1),
    'SLIDING_TOKEN_LIFETIME': timedelta(days=30),
    'SLIDING_TOKEN_REFRESH_LIFETIME_LATE_USER': timedelta(days=1),
    'SLIDING_TOKEN_LIFETIME_LATE_USER': timedelta(days=30),
}
```
# Step 6: Token Endpoints
Include Simple-JWT token endpoints in your project’s urls.py:

```
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),    
    path('', include('myapp.urls')), # include your app urls.py here
]
```
# Step 7: Create API Views
Create your API views in `myapp/views.py` within your app. These are endpoints that will require authentication.

```
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from rest_framework_simplejwt.authentication import JWTAuthentication



class Home(APIView):
    authentication_classes = [JWTAuthentication]
    permission_classes = [IsAuthenticated]

    def get(self, request):
        content = {'message': 'Hello, World!'}
        return Response(content)
````

# Step 8: URL Configuration
First create myapp/urls.py file in your app. Then configure your URLs in myapp/urls.py within your app:
```
touch myapp/urls.py
```
```
from django.urls import path
from .views import Home


urlpatterns = [
    path('', Home.as_view()),
]
```
# Step 9: Running Migrations
```
python manage.py migrate
```
# Step 10: Testing

```
python manage.py createsuperuser
python manage.py runserver
```

![Image description](https://github.com/oovnath/django_jwt/blob/main/img/image.png?raw=true)

![Image description](https://github.com/oovnath/django_jwt/blob/main/img/image2.png?raw=true)