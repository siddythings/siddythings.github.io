---
author : "Sidhart"
title : "Dynamic URLs with Flask | Ep. 08"
# date : "2020-09-07"
description : "Learn how to link nevbar and create and work with dynamic URLs and dynamic data in Flask"
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

Hii, 
In this segment, we are learning about mapping routes to a navbar and create dynamic URLs.
<!--more-->

Dynamic URLs play a very important role in the ability to create unique URLs that aren't hardcoded into our application.

For example, let's say you create an application that allows users to create an account and log in to their profile, we'll need a way to dynamically generate a route for that specific user.

### Linking navbar

Link your navbar to your routes 
- Using your URL path  ```href="/blog"```
- Using ```url_for('<function>')``` ```e.g. href="{{url_for('todo')}}"```

```<root>/application/templates/public/navbar.html Open navbar.html```

```
<ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="/">Home <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item active">
                    <a class="nav-link" href="/blog">Blog <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item active">
                    <a class="nav-link" href="/about">About <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item active">
                    <a class="nav-link" href="{{url_for('todo')}}">Todo <span class="sr-only">(current)</span></a>
                </li>
</ul>
```

### Creating a dynamic route

Lets first create a sign in route in our ```views.py``` file
```py
@app.route("/signin")
def signin():
    return render_template("public/main/signup.html")
```
Also, create a new template signup.html
```html
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
    <h1 class="mt-5">Profile</h1>
</main>
{% endblock %}
```
At this movement, your URL isn't dynamic. We need to make some changes to our ```@app.route``` decorator.

We are adding a variable in the decorator which can take values as input from the user.

```@app.route("/signup/<username>)```
- We've added trailing slash to ```/signup/```
- We've provided a variable in the URL path and wrapped in with two opposing arrows ```<>``` as shown.
username is a variable which can store dynamic values 
Now we need to make some changes to our ```views.py```

```@app.route('/signin/<username>')```
```py
def signin(username):
  return render_template("public/main/profile.html",username=username)
```
Now, we are using the username variable to pass in the function as an argument and sending back to your HTML page as an object

I hope you can understand the cycle of data flow, we are sending data from URL to page.

Now we need to add an object in the HTML page.
```html
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
    <h1 class="mt-5">Profile</h1>
   <h3> Hello {{username}} </h3>
</main>
{% endblock %}
```
After all, these changes save it.

Start your application.

```And Hit :-``` [127.0.0.1:5000/signin/siddythings](127.0.0.1:5000/signin/siddythings)

You'll see something like this in your window

```Hello siddythings ```

```Tip:``` Try to put your name in the place of ```siddything```.

### Multiple dynamic variables

Open ```views.py``` file

```@app.route("/multiple/<name>/<age>/<country>")```

```py
def multiple(name,age,country):
	return render_template("public/main/multipal.html",name=name,age=age,country=country)
```
Create ```multipal.html``` file
```html
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
    <h1 class="mt-5">Profile</h1>
    <h3>Name : {{name}}</h3>
    <h3>Age : {{age}}</h3>
    <h3>Country : {{country}}</h3>
</main>
{% endblock %}
```
Save everything and restart your application

Hit:- [http://127.0.0.1:5000/multiple/siddythings/23/USA](http://127.0.0.1:5000/multiple/siddythings/23/USA)



### Wrapping up 

In this segment we have learn about dyanamic urls and linking navbar.
Try to do some project yourself

In the upcoming segment we are learing about linking Database to our application.


##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)