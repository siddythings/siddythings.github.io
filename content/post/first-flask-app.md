+++
author = "Hugo Authors"
title = "Your First Flask Applaction | Ep. 01"
date = "2020-09-02"
description = "In first episode of learing we are trying to create and run very first Flask Applaction"
tags = [
    "python",
    "Flask",
    "web development",
]
categories = [
    "Flask",
    "Flask Beginner",
]
series = ["Flask Learing Guide"]
aliases = ["flask-learing-guide"]
+++



Im Glad you think to learn Flask.

In this part, you will be creating a simple web application with minimum requirements.

We're going to use a virtual environment throughout this series of learning. 
<!--more-->
## Creating the project directory and the virtual environment.

First of all, we need to create a new project folder.
Tip:- You can use your project name So it's easy to follow along

### Creating Project Folder
```shell
mkdir flask-application
```
Note: If you are a windows user you can simply create a folder.

Next, let's go inside to flask-application or the folder you just created.

Now we need to create a Python virtual environment.
```shell
python -m venv env
```
This will create a new virtual environment called env. You can see a folder just created inside your flask-application folder called env. Now you need to activate your virtual environment before you install Flask in your system.
```shell
source env/bin/activate
```
You'll see (env) appear in front of your terminal prompt. This indicates that your virtual environment is activated.

Installing Flask
You can simply install Flask.
```shell
pip install flask
```
You can simply list down your package install in your environment
```shell
pip list

Package       Version
------------- -------
click         7.1.2  
Flask         1.1.2  
itsdangerous  1.1.0  
Jinja2        2.11.2 
MarkupSafe    1.1.1  
pip           9.0.1  
pkg-resources 0.0.0  
setuptools    39.0.1 
Werkzeug      1.0.1 
```
OK, So all boring part is done now you can start your rehearsal.

### Creating a Flask app

This episode is to guide you to create a simple web app. You'll get to know how to structure your Flask Application in the upcoming series.

Let's create a file name app.py inside your main(in my case flask-application) folder.
```shell
from flask import Flask
app = Flask(__name__)
@app.route("/")
def index():
    return "Hello world!"
if __name__ == "__main__":
    app.run()
```
Just copy the above code.

What exactly happening is first of all we just import the flask package.
```
from flask import Flask
app = Flask(__name__)
```
After that, we create our Flask Application. So we are passing the __name__ to Flask and assign it to the variable app.

Don't worry about exactly why we're doing this. We'll cover it in a more advanced episode in this series.
```shell
@app.route("/")
def index():
    return "Hello world!"
```
Routes in Flask are created using the @app.route decoder and passing in a URL.

Example:- We've passed "/" into the @app.route decorator. "/" is the root of the website.
This route will be triggered when someone hit on the index of the website. e.g., http://example.com
```shell
def index():
    return "Hello world!"
```
Under @app.route("/") decorator, we simply write the function of that route, which means what happens when someone hits the index of the website.
In this case, you'll see "Hello world!" in the window.

Every application needs a server by which the application can start your webserver.
```shell
if __name__ == "__main__":
    app.run()
```
This helps to start your server.


### Running the Flask App

Now here comes the movement you want to see the magic of your code !!

in your terminal, make sure you're in the same folder as app.py and run this line of code.
```shell
python app.py
```
You'll see the following message in your terminal to let you know Flask is running:
```
* Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:8000/ (Press CTRL+C to quit)
```
Great, You have successfully run your first flask application.
open a new tab in your browser and hit this URL.

[http://127.0.0.1:5000/](http://127.0.0.1:5000/)

You should see Hello world! in your browser!

### Setting Up Flask environment variables

To make it easier to run your flask application, You just need to add your application file to the environment variable. Run the following commands and we'll talk through them after:
```shell
export FLASK_APP=app.py
export FLASK_ENV=development
```
Running export FLASK_APP=app.py will set the FLASK_APP variable to app.py
Running export FLASK_ENV=development tells Flask we want to run our app in development mode

Imp Note - Never run a live Flask application in production using development mode

We can now run our app using the following simple command:
```shell
flask run
```
You'll see
```
* Serving Flask app "app.py" (lazy loading)
 * Environment: development
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 246-610-309
```

To deactivate your virtual environment, simply enter:

```shell
deactivate
```

### Wrapping Up

You now know how to  create and run a basic flask application and how to setup environment for your application
