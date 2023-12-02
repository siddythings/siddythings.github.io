---
author : "Sidhart"
title : "Serialization - Django REST framework| Ep. 02"
# date : "2020-10-03"
description : "Django, API, REST,Serialization - Django REST framework"
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


In this tutorial, we'll be covering creating a simple Web API. 

Along the way, it will introduce the various components that make up the REST framework and give you a comprehensive understanding of how everything fits together. 
<!--more-->

### Introduction 

Serializers are responsible for converting python objects into data types understandable by front-end frameworks and vice versa.

#### Getting started

Let's start coding. To get started, let's create a new project.
```
django-admin startproject djserializers
cd djserializers
```

Now create an app 
```
python manage.py startapp todo
```

We'll need to app your app to your ```INSTALLED_APPS```.
Open ```djserializers/settings.py```

```
INSTALLED_APPS = [
    ...
    'rest_framework',
    'todo',
]
```

#### Creating a ```model``` for ```todo``` app

Let's create a model for out todo app by creating a simple ```Todos``` model that is used to store todos.

Open ```todo/models.py```
```py
from django.db import models

# Create your models here.
class Todos(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    content = models.TextField()

    class Meta:
        ordering = ['created']
```

We'll also create an ```migration``` for our todo model, and ```sync``` the database.
```
python manage.py makemigrations snippets
python manage.py migrate
```

#### Creating a Serializer class

Web API is to provide a way of serializing and deserializing the todo instances into representations such as ```JSON```

Create a file in the todo folder named ```serializers.py``` and add the following.
```py
from rest_framework import serializers
from .models import Todos

class TodoSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_black=True, max_length=100)
    content = serializers.CharField(style={'base_template': 'textarea.html'})

    def create(self, validated_data):
        """
        Create and return a new `Todos` instance, given the validated data.
        """
        return Todos.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """
        Update and return an existing `Todos` instance, given the validated data.
        """
        instance.title = validated_data.get('title', instance.title)
        instance.content = validated_data.get('content', instance.content)
        instance.save()
        return instance
```

A serializer class is very similar to a ```Django Form``` class, and includes similar validation flags on the various ```fields```, such as ```required```, ```max_length``` and ```default```.

#### Writing regular Django views
```py
from django.shortcuts import render
# Create your views here.
from django.http import HttpResponse, JsonResponse
from django.views.decorators.csrf import csrf_exempt
from rest_framework.parsers import JSONParser
from .models import Todos
from .serializers import TodoSerializer

@csrf_exempt
def todos_list(request):
    if request.method == 'GET':
        todo = Todos.objects.all()
        serializer = TodoSerializer(todo, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = TodoSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data, status=201)
        return JsonResponse(serializer.errors, status=400)


@csrf_exempt
def todo_detail(request, id):
    try:
        todo = Todos.objects.get(pk=id)
    except Todos.DoesNotExist:
        return HttpResponse(status=404)

    if request.method == 'GET':
        serializer = TodoSerializer(todo)
        return JsonResponse(serializer.data)

    elif request.method == 'PUT':
        data = JSONParser().parse(request)
        serializer = TodoSerializer(todo, data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        return JsonResponse(serializer.errors, status=400)

    elif request.method == 'DELETE':
        todo.delete()
        return HttpResponse(status=204)
```

Then we need to bind these views. Create a file ```urls.py``` in ```todo``` folder
```py
from django.urls import path
from todo import views
urlpatterns = [
    path('todo/', views.todos_list),
    path('todo/<int:id>', views.todo_detail),

]
```
Now bind app ```urls.py``` to site ```urls.py```
```py
from django.urls import path, include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('todo.urls')),
]
```

#### Testing Web API
Startup Django's development server.
```s
python manage.py runserver

Validating models...

System check identified no issues (0 silenced).
October 07, 2020 - 07:22:22
Django version 3.1.2, using settings 'djserializers.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
Finally, we can get a list of all of the todos:
```s
curl http://127.0.0.1:8000/todo/
[
{
"id": 1,
 "title": "Homework",
 "content": "Do Homework"
}
{
"id": 2,
 "title": "Food",
 "content": "Do shoping"
}
]
```
Or we can get a particular todo by referencing its id:
```s
curl http://127.0.0.1:8000/todo/1
[
{
"id": 1,
 "title": "Homework",
 "content": "Do Homework"
}
]
```
### Wrapping Up
We're doing okay so far,we've got a serialization API that feels pretty similar to Django's Forms API, and some regular Django views.

For more details [Django REST](https://www.django-rest-framework.org/tutorial/1-serialization/)

##### If you stuck someware checkout my code
[Github](https://github.com/Apex1000/django-blogs)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)