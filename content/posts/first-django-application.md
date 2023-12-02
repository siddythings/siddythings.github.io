---
author : "Sidhart"
title : "Your First Django Application | Ep. 01"
# date : "2020-10-02"
description : "In first episode of learing we are trying to create and run very first Django Applaction"
tags : [
    "python",
    "Django",
    "web development",
]
categories : [
    "Django",
    "Django Beginner",
]
# series : ["Django Learing Guide"]
aliases : ["django-learing-guide"]
---


In this part, you will be creating a simple REST application with minimum requirements.
We’re going to use a virtual environment throughout this series of learning.
<!--more-->

Creating the project directory and the virtual environment.
First of all, we need to create a new project folder. 

```Tip:-``` You can use your project name So it’s easy to follow along

Creating Project Folder
Project setup
Create a new Django project named tutorial, then start a new app called ```quickstart```.


### Create the project directory
```py
mkdir tutorial
cd tutorial
```

#### Create a virtual environment to isolate our package dependencies locally
```py
python3 -m venv env
source env/bin/activate  # On Windows use `env\Scripts\activate`
```
#### Install Django and Django REST framework into the virtual environment
```
pip install django
pip install djangorestframework
```
#### Set up a new project with a single application
```
django-admin startproject tutorial .
django-admin startapp quickstart
cd ..
```

The project layout should look like:
```py
.
├── quickstart
│   ├── migrations
│   ├── __init__.py
│   ├── admin.py
│   ├── app.py
│   ├── models.py
│   ├── serializers.py
│   ├── tests.py
│   └── views.py
├── tutorial
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── env
└── manage.py
```
Now sync your database for the first time:

```
python manage.py migrate
```

We'll also create an initial superuser named ```admin``` with a password of ```password123```.
```
python manage.py createsuperuser --email admin@example.com --username admin
```



Create a model in the database that Django ORM will manage
Let’s make our first model!
```py
# models.py
from django.db import models
class Users(models.Model):
    name = models.CharField(max_length=60)
    alias = models.CharField(max_length=60)    
    def __str__(self):
        return self.name
```
#### Migrate the database
```
python manage.py makemigrations
python manage.py migrate
```

#### Register Users with the admin site

Open ```quickstart/admin.py``` and make it look like this:
```py
from django.contrib import admin
from .models import Users
admin.site.register(Users)
```

#### Serialize the Users model
```py
from rest_framework import serializers

from .models import Users

class UsersSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Users
        fields = ('name', 'alias')
```


#### Display the data

##### Views
```py
from rest_framework import viewsets

from .serializers import UsersSerializer
from .models import Users


class UserView(viewsets.ModelViewSet):
    queryset = Users.objects.all().order_by('name')
    serializer_class = UserSerializer
```

Site URLs
```py
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('quickstart.urls')),
 ]
```
Quickstart URLs
```py
from django.urls import include, path
from rest_framework import routers
from . import views

router = routers.DefaultRouter()
router.register(r'users', views.UsersView)

urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```

### Wrapping Up

You now know how to  create and run a basic django-rest application and how to setup 
environment for your application


##### If you stuck someware checkout my code
[Github](https://github.com/Apex1000/django-blogs)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)