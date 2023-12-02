---
author : "Sidhart"
title : "Creating forms with Flask | Ep. 07"
# date : "2020-09-06"
description : "Creating forms, posting data to views and working with form data in Flask"
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
In this segment, we are going to talk about Forms.
Forms are the way to take user inputs. 
You'll learn about post form to a flask view and work with incoming data.
<!--more-->

##### In this example, We are creating a simple todo app user form.

### Creating a new route

Create a new route in ```views.py```

```<root>/application/views.py```
```
@app.route("/todo")
def todo():
    return render_template("public/todo.html")
```
We're going to accepting ```POST``` request from the form so we need to pass another argument to ```@app.route```

we do this by passing methods argument, along with a list of methods as value

Flask supports all of the common HTTP methods including:
- GET
- POST
- PUT
- DELETE

So let's add methods in the ```@app.route```
```py
@app.route("/todo", methods=['GET','POST')
def todo():
    return render_template("public/main/todo.html")
```

```Tip:-``` If you don't know about [HTTP methods]()

```Note:-``` Flask routes support ```GET``` requests by default, but if you need to add any specific method then you have to add those methods.

Now Let's create templates for views

```<root>/application/public/main/todo.html```

```html
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
    <h3>Todo App</h3>
</main>
{% endblock %}
```
Save the page and go to localhost:5000/todo 
you'll see the page

Now add a form
```html
<form action="/todo" method="POST">
        <div class="form-group">
          <label>Task</label>
          <input type="text" class="form-control" id="username" name="task" placeholder="name">
        </div>
        <button type="submit" class="btn btn-primary">Add Task</button>
</form>
```
```Notice:``` ```action='/todo'``` and ```method='POST'```

The ```action``` tells the form where to send user data, and the ```method``` tells what type of request form makes

Your templates will look like

```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
    <h3>Todo App</h3>
    <form action="/todo" method="POST">

        <div class="form-group">
          <label>Task</label>
          <input type="text" class="form-control"  name="task" placeholder="Add Task">
        </div>
        <button type="submit" class="btn btn-primary">Add Task</button>
      </form>
      
</main>
{% endblock %}

```

Save it and refresh the page now you will see the form try to add any task.

It doesn't show anything because we haven't handled the form data from the ```views.py``` 

Open ```views.py```

We need to add a few things to our ```views.py``` file 

- import request for handling the form request by adding from flask import   redirect, request
- also, add request handler means when there is a POST request then how the views react.

##### Request handler
```py
@app.route("/todo", methods=['GET','POST')
def todo():
    if request.method == "POST":
      name = request.form['task']
      return render_template("public/main/todo.html",task=name)
    return render_template("public/main/todo.html",task=name)
```
As you can see

There is an if condition which is just checking the type of request.
```if request.method == "POST"```

we have added a name tag in our ```HTML``` input form by which you can access the value of that input field.

```value = request.form['task']```

Till now user can send the data to the flask views but can't see it, we need to send back the data by which the user can see the task list
So we send the data object

```py
return render_template("public/main/todo.html",task=name)
```
After all this 
```views.py```
```py
@app.route("/todo", methods=["GET","POST"])
def todo():
    if (request.method == "POST"):
      value = request.form["task"]
      return render_template("public/main/todo.html",task=value)
    return render_template("public/main/todo.html")
```

```todo.html```
```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
  <h3>Todo App</h3>
  <form action="/todo" method="POST">

    <div class="form-group">
      <label>Task</label>
      <input type="text" class="form-control" name="task" placeholder="Add Task">
    </div>
    <button type="submit" class="btn btn-primary">Add Task</button>
  </form>
  <br>
  <p>{{task}}</p>
</main>
{% endblock %}
```

But in this, we can not add more than 1 task so we need something which can store more than one record like a list

```py
tasks = [] //global list
@app.route("/todo", methods=["GET","POST"])
def todo():
    if (request.method == "POST"):
      value = request.form["task"]
      return render_template("public/main/todo.html",task=value)
    return render_template("public/main/todo.html")
```
Here tasks act as a database that stores values on a temporary basis.

```todo.html```

```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
  <h3>Todo App</h3>
  <form action="/todo" method="POST">

    <div class="form-group">
      <label>Task</label>
      <input type="text" class="form-control" name="task" placeholder="Add Task">
    </div>
    <button type="submit" class="btn btn-primary">Add Task</button>
  </form>
  <br>
  {% for task in tasks%}
  <ul>
    <li>{{task}}</li>
  </ul>
  {% endfor %}
</main>
{% endblock %}
```

### Wrapping up 

In this segment we have learn user inputs.
Try to do some project yourself

In the upcoming segment we are learing URL, routes


##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)