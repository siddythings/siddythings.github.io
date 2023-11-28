---
author : "Sidhart"
title : "Jinja Templating Flask | Ep. 05"
date : "2020-09-04"
description : "Jinja Templating in Flask"
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
In the last segment, we talk a bit about ```Jinja templating```. So in this segment, we are going to learn about How to work with ```Jinja templating``` engine with Html.
<!--more-->

I assume you are familiar with the concept of [```Inheritance```](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)). 

Template inheritance work's also the same. We create a series of ```base templates``` and import them into ```child templets```, In the concept of not repeating the same code again and again.

#### Let's talk with an example:-
Every website has these components.
- header
- navbar
- main content
- footer

And if you create 4 pages
- Home page
- About page
- Blog page
- Contact us page

So, in all these 4 page's only content will differ but the rest of the components will be the same.

Template inheritance help in writing the clean code.

Let's get coding!

### Creating base templates

If you have followed in the last segment then you understand the concept of breaking up our Flask application into sub-directories or separate folder/file structure to maintain good readability of the code.

So, In this segment, we are creating a file structure for the HTML template

Let's create folders for HTML.
Open the templates folder
```<root>/application/templates```
- Step 1: Go inside the admin folder
- Step 2: Delete All the files we created in the last segment.
- Step 3: Create two folders layout and main inside the admin.
- Step 4: Create base.html, footer.html, and nevbar.html inside the layout folder.
- Step 5: Create index.html inside the main folder. 

Repeat the same for the public folder also.


#### Folder tree

After that, your structure looks like this.
```
.
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   ├── static
│   │   ├── css
│   │   │   └── style.css
│   │   ├── images
│   │   │   └── img.jpg
│   │   └── js
│   │       └── app.js
│   ├── templates
│   │   ├── admin
│   │   │   └── layout
│   │   │   │   ├── base.html
│   │   │   │   ├── footer.html
│   │   │        └── nevbar.html
│   │   │   ├──main
│   │   │       └── index.html
│   │   └── public
│   │   │   └── layout
│   │   │   │   ├── base.html
│   │   │   │   ├── footer.html
│   │   │        └── nevbar.html
│   │   │   ├──main
│   │   │       └── index.html
│   └── views.py
├── requirements.txt
└── wsgi.py
```
As you can see we have created the different layout and main folders for public and admin.

Both the layout and main folder has file so you can custom design for different users.

In the ```base.html``` contents all the ```stylesheet```, ```js```, and all including files.

Open ```base.html``` file

```<root>/application/templates/public/layout/base.html```
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="https://getbootstrap.com/docs/4.0/assets/img/favicons/favicon.ico">
    <title>{{ title }}</title>
    <link rel="canonical" href="https://getbootstrap.com/docs/4.0/examples/sticky-footer-navbar/">
    <!-- Bootstrap core CSS -->
    <link href="https://getbootstrap.com/docs/4.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Custom styles for this template -->
    <link href="https://getbootstrap.com/docs/4.0/examples/sticky-footer-navbar/sticky-footer-navbar.css"
        rel="stylesheet">
</head>
<body>
    <!-- Begin page content -->
    {% include "public/layout/navbar.html"%}
    {% block content %}{% endblock %}
    {% include "public/layout/footer.html"%}
    
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"
        integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN"
        crossorigin="anonymous"></script>
    <script>window.jQuery || document.write('<script src="../../assets/js/vendor/jquery-slim.min.js"><\/script>')</script>
    <script src="https://getbootstrap.com/docs/4.0/assets/js/vendor/popper.min.js"></script>
    <script src="https://getbootstrap.com/docs/4.0/dist/js/bootstrap.min.js"></script>
</body>
</html>
```
Notice:- 
``{{ title }} ``: Variable from flask views

``` {% include "public/layout/navbar.html"%} ```: inherit nevbar.

```{% block content %}{% endblock %}```: inherit content from the child templates.

```{% include "public/layout/footer.html"%}```: inherit footer.


### Child templates

Open ```index.html``` file
```<root>/application/templates/public/main/index.html```
```
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
    <h1 class="mt-5">Index page</h1>
</main>
{% endblock %}
```
Open ```views.py``` files

```<root>/application/views.py```
```python
from flask import render_template
from application import app
@app.route('/')
def index():
    return render_template('public/main/index.html',
                            title="Flask Application")
```
```Notice```:-  sending title from flask to HTML as you saw above.

#### Assessment

- Make about page, blog page.
- Make the Admin panel.

Hint :
- Modify about, blog routes and add HTML page for it as same as ```index.html```
- Add Admin panel
  - add routes in admin_views.py
  - add templates in layout and main folder

### Wrapping up

Don't worry it's not complicated you just need some time and revision you can do it!

##### If you stuck someware checkout my code

[Github](https://github.com/Apex1000/flask-blog)