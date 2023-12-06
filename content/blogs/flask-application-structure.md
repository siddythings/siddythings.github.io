---
author : "Sidhart"
title : "Flask application structure | Ep. 02"
# date : "2020-09-03"
description : "In this part well looking at structure of your application"
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


This part is about how to structure your application and create new routes. You'll learn about the different structure of your application.  
<!--more-->
Flask is one of the most flexible frameworks unlike any other MVT(e.g. Django). 

So it's very important, To structure your application. 
Because when you are dealing with multiple features in an application then structure will very helpful. 
There are many ways to set up your application structure. This pattern I've been using from the last couple of years. So it's very easy to understand and work with.

### Your application as a package
I'm sure you are well aware of python packages. So by far, the most popular way of structure your application is a package method, where you can define your application as a package and you can import it! Just like any other python package.

Let's jump into code!

So, Let's look at how our new structure looks like. 
```
.
├── application
│   ├── __init__.py
│   └── views.py
├── env
├── requirements.txt
└── wsgi.py
```
### Step-by-step project structure.

Go inside your folder and remove the previous files you create in the last series.

Tip: Don't delete the environment folder env you need that to activate your environment in this series also.

- Step 1: Create a file wsgi.py
- Step 2: Create a folder application 
- Step 3: Create ```__init__.py``` and views.py inside your application folder.

Done!

Don't worry about the requrements.txt file.

Now, Open your favorite IDE.

Open ```__init__.py``` file
```~/<DIR>/application/__init__.py```

```
from flask import Flask
app = Flask(__name__)
from application import views
```
You're familiar with the first two lines. However, you'll think about the third line.

Think like ```__init__.py``` file as a constructor that pull all the part of the application and build it together.
And then tells Python to treat it as a package!

Now let's add some views.

Open ```views.py``` file
```~/<DIR>/application/views.py```

```
from application import app
@app.route('/')
def index():
    return "Index Page"

@app.route('/about')
def about():
    return "About Page"

@app.route('/blog')
def blog():
    return "Blog Page"
```

Same as our first app, we're creating some routes using the ```@app.route``` decorator and passing URL.
The only difference is from the application import app.
Actually, we're simply importing app variable from ```__init__.py``` file

Open ```wsgi.py``` file
```~/<DIR>/wsgi.py```

```
from application import app
if __name__ == '__main__':
	app.run()
```

Same as ```views.py```  file we are importing application (python treats as a package so we can import it.) 
And we are calling the ```app.run()``` method, just like our first app.

Now, You see everything is the same as our first app but now everything is in a different file.

### Flask environment variable

If you have deactivated your project environment the activate it
Hit the following command from your parent folder.
```source env/bin/activate ```

Same as in the first app Hit : 
```
export FLASK_APP=wsgi.py
export FLASK_ENV=development
```
Running your app

Run your application with the following command.
```
flask run
```
You'll see the following message, just like this 
```
* Serving Flask app "run.py" (lazy loading)
 * Environment: development
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 137-205-426
```
Now Open a new tab in the browser 

Hit:- [http://127.0.0.1:5000/](http://127.0.0.1:5000/)

You'll see "Index Page".

Now Hit:- [http://127.0.0.1:5000/about](http://127.0.0.1:5000/about)

You'll see "About Page"

Now Hit:- [http://127.0.0.1:5000/blog](http://127.0.0.1:5000/blog)

You'll see "Blog Page"

You see we just create three different routes.

So, let's do a level-up

Create a file ```admin_views.py``` inside your application folder

This file contains all the routes of admin Handel.
```
from application import app

@app.route('/admin/dashboard')
def admin():
    return "Welcome Admin"
```
Now you created all the admin routes but your application doesn't know about it because you haven't added to your package.

Open ```__init__.py``` file and add from application import admin_views
at the end of your file
```
from flask import Flask
app = Flask(__name__)
from application import views
from application import admin_views
```
Save everything and run the application.

Now Hit: [http://127.0.0.1:5000/admin/bashboard](http://127.0.0.1:5000/admin/bashboard)

You'll see "Welcome Admin"
Hit ```Ctrl+C``` to close the application.

Requirements
From the parent folder run the following command:
```
pip freeze > requirements.txt
```
This command creates a file name requirements.txt and stores every package you install in your environment.

when you open the file you'll see something like this:-
```
click==7.1.2
Flask==1.1.2
itsdangerous==1.1.0
Jinja2==2.11.2
MarkupSafe==1.1.1
pkg-resources==0.0.0
Werkzeug==1.0.1
```
It same as when you hit the pip list. Just the difference is this will create a file in your application and this will help you at the time of hosting your application.

Tip - To install packages from a requirements.txt file, run ```pip install -r requirements.txt``` 
### Wrapping Up
At the end you file structure will look like this.
```
.
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   └── views.py
├── env
├── requirements.txt
└── wsgi.py
```

##### If you stuck someware checkout my code
[Github](https://github.com/Apex1000/flask-blog)