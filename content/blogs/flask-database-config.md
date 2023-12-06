---
author : "Sidhart"
title : "Flask Database with Flask-SQLAlchemy  | Ep. 10"
# date : "2020-09-09"
description : "Connect Flask to a Database with Flask-SQLAlchemy"
tags : [
    "python",
    "Flask",
    "web development",
]
categories : [
    "Flask",
    "Flask Beginner",
]
# series : ["Flask Learing Guide"]
aliases : ["flask-learing-guide"]
---

Hello, 

In this segment, we are going to look at how to interact with the database in the flask.
<!--more-->

BTW Flask can easily bond with many databases. So, you can choose your own 
But in this segment, we are using ```Flask-SQLAlchemy``` as our database.

Lots of trash talks lets get started.

### Installing the Flask-SQLAlchemy
```$ pip install -U Flask-SQLAlchemy```

```pip list ```
```
Package          Version    
---------------- -----------
click            7.1.2      
config           0.5.0.post0
Flask            1.1.2      
Flask-SQLAlchemy 2.4.4      
itsdangerous     1.1.0      
Jinja2           2.11.2     
MarkupSafe       1.1.1      
pip              9.0.1      
pkg-resources    0.0.0      
setuptools       39.0.1     
SQLAlchemy       1.3.19     
Werkzeug         1.0.1      
```

### Configuring Flask-SQLAlchemy For Your Application

There are a few ```configuration variables``` we need to set up before interacting with our database.

Open ```config.py```

```<root>/config.py```

```py
from os import environ, path
from dotenv import load_dotenv
import os
basedir = path.abspath(path.dirname(__file__))
load_dotenv(path.join(basedir, '.env'))
class Config(object):
    """Base config."""
    SECRET_KEY = "qeqwrwe1231weerw"
    STATIC_FOLDER = 'static'
    TEMPLATES_FOLDER = 'templates'
SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or \
        'sqlite:///' + os.path.join(basedir, 'database.db')
    SQLALCHEMY_TRACK_MODIFICATIONS = False
    DEBUG = True

class Production(Config):
    FLASK_ENV = 'production'
    DEBUG = False
    TESTING = False
   
class Development(Config):
    FLASK_ENV = 'development'
    DEBUG = True
    TESTING = True
```

There are a few variables here which are specific to ```Flask-SQLAlchemy```:
- ```SQLALCHEMY_DATABASE_URI```: the connection string we need to connect to our database.
- ```SQLALCHEMY_TRACK_MODIFICATIONS```: I just always set this to False.

At this movement, we haven't created our database file
Create a file database.db in you ```<root>``` folder

```
.
├── /application
├── /env
├── config.py
├── database.db
├── requirements.txt
└── wsgi.py
```

### Initiating Flask-SQLAlchemy

In this, we are using the ```Flask Application Factory``` method for initiating our app.

First of all, we need to ```import SQLAlchemy```

```from flask_sqlalchemy import SQLAlchemy```

and now we need to create an SQLAlchemy to an object so we can use our database

```db = SQLAlchemy()```


Open ```__init__.py```
```<root>/application/__init__.py```

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

db = SQLAlchemy()

def create_app():
    if app.config["ENV"] == "production":
        app.config.from_object("config.Production")
    else:
        app.config.from_object("config.Development")
    db.init_app(app)
    with app.app_context():
        from application import views
        from application import admin_views
        db.create_all()  # Create sql tables for our data models
    return app
```

You have noticed ```create_app()```

Having wrapped the app object inside of the create_app function, we still need a way to access it other than calling the function itself.

Now we need to import the create_app function

Open ```wsgi.py ```
```py
from application import create_app 
app = create_app()

if __name__ == '__main__':
    app.run()
```
Creating Database Models
Create a models.py file in our application directory.
```import``` the ```db``` object that we created in``` __init__.py```

```
.
├── /application
│   ├── static
│   ├── templates
│   ├── __init__.py
│   ├── adminviews.py
│   ├── models.py
│   └── views.py
├── /env
├── config.py
├── database.db
├── requirements.txt
└── wsgi.py
```
open ```model.py ```

```py
from . import db
class Todos(db.Model):
    __tablename__ = 'todos'
    id = db.Column(
        db.Integer,
        primary_key=True
    )
    todo = db.Column(
        db.String(64),
        index=False,
        unique=True,
        nullable=False
    )
    def __repr__(self):
        return '<Todo {}>'.format(self.todo)
```

### Creating Our First Todo

Let's create new routes in our ```views.py```
Firstly import models and db object

```from .models import db, Todos```

And add a route to add tasks in the database

```py
@app.route('/todo/<task>')
def todos(task):
  new_todo = Todos(todo = task)
  db.session.add(new_todo)  # Adds new Todo record to database
  db.session.commit()  # Commits all changes
  return "<h1> You have successfully created your task : "+task+"</h1>"
```

Save everything and start your application

And run application

```flask run```

Done!!

Hit: [http://127.0.0.1:5000/todo/siddythings](http://127.0.0.1:5000/todo/siddythings)


### Wrapping up 

In this segment we have learn about seting up the database.

Try to do some project by yourself.

In the upcoming segment we are learing about showing querying the records.


##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)