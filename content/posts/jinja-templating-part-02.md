---
author : "Sidhart"
title : "Jinja Templating Flask Part-02 | Ep. 06"
date : "2020-09-06"
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
This segment is going to a bit more about Jinja templating. 
In the last segment, you have to learn how to create base templates, child templates, and how to extend and include templates.
<!--more-->
But there are lots of things to learn. So let's get started.

I hope you have done the assessment.

If not 
Let's create view's 

```<root>/application/views.py```
```python
@app.route('/about')
def about():
    return render_template('public/main/about.html'
						title="About Page")

@app.route('/blog')
def iblog():
    return render_template('public/main/blog.html',
					title="Blog Page")
```
Also, sending custom title for the Page Title as objects

```<root>/application/templates/public/about.html```
```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
    <h1 class="mt-5">About page</h1>
</main>
{% endblock %}
```

```<root>/application/templates/public/blog.html```
```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
    <h1 class="mt-5">Blog page</h1>
</main>
{% endblock %}
```

#### So let's talk about passing object 

As you have seen in the previous segment we are passing objects from views to the ```HTML page```.
So, there are some rules in ```Jinja templating```.

#### Single Variable
```python
@app.route("/variable")
def variable():
    my_name = "xyz"
    return render_template("public/main/variable.html",
						name=my_name)
```
In the above case, my_name contains the value, and we passing that value to the name object. Which can be accessed by the HTML page with name object
```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
   <h3 > Hello {{ name }} </h3>
</main>
{% endblock %}
```
As you see in the above, for example, we are excessing that object by there name.

Note:- Always use ```{{ <object name> }}``` like this.

What happens if we pass an array as an object.


#### List variable
Then we simply use a loop to print all there value.
```python
@app.route("/variable")
def variable():
    my_name = ["Tom","Jarry","Oggy","cockroaches"]
    return render_template("public/main/variable.html",
				names=my_name)
```

```html
{% extends "public/layout/base.html" %}
{% block content %}

<main role="main" class="container">
{% for name in names %}   
<h3 > Hello {{ name }} </h3>
{% endfor %}
</main>
{% endblock %}
```

As you can see we are using for loop to iterate in our names array. And also notice the syntax of the for loop we using ```{% loop index %}``` and also end ```{% end loop %}```
 it some things like C-lang it has open and close statements

#### Dictionaries
```python
prise = {
    "water": 20,
    "colddrink": 30,
    "softdrink": 25,
    "mango": 20,
    "nuts": 34
}
```

```py
@app.route("/variable")
def variable():
    prise = {
    "water": 20,
    "colddrink": 30,
    "softdrink": 25,
    "mango": 20,
    "nuts": 34
		}
    return render_template("public/main/variable.html",
				prises=prise)
```
Accessing object by indexes, key, and attributes
```h
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
<h3 > Hello {{ prises.water }} </h3>
</main>
{% endblock %}
```
Accesing all objects by loop 
```h
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
{% for name, prise in prises.items() %}
<h3 >the cost of {{ name }} is {{ prises }}. </h3>
{% endfor %}
</main>
{% endblock %}
```

#### Conditionals & comparison operators

numbers = [1,2,3,4,5,6,7,8,9,10]
Print only even numbers
```py
@app.route("/variable")
def variable():
    numbers = [1,2,3,4,5,6,7,8,9,10]
    return render_template("public/main/variable.html",
				numbers=numbers)
```

```h
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
{% for i in numbers%}
{% if i%2==0 %}
<h3 >even {{i}}. </h3>
{% endif %}
{% endfor %}
</main>
{% endblock %}
```

```Notice```:- Every block start and ends by there name.


Sumup all code .

Create a new view in views.py
```<root>/application/views.py```

```py
@app.route('/variable')
def variable():
    my_name = "xyz"
    listname = ["Tom","Jarry","Oggy","cockroaches"]
    prise = {
    "water": 20,
    "colddrink": 30,
    "softdrink": 25,
    "mango": 20,
    "nuts": 34
    }
    numbers = [1,2,3,4,5,6,7,8,9,10]

    return render_template('public/main/variable.html',
                            name =my_name,
                            listname =listname,
                            prises =prise,
                            numbers =numbers)
```
Add new variable templates.

```<root>/application/templates/public/main/variable.html```

```h
{% extends "public/layout/base.html" %}
{% block content %}
<main role="main" class="container">
    <h3>Single Variable</h3>
    <h5> Hello {{ name }} </h5>
    <hr>
    <h3>List Variable</h3>
    {% for name in listname %}
    <h5> Hello {{ name }} </h5>
    {% endfor %}
    <hr>
    <h3>Dictionaries</h3>
    {% for name, prise in prises.items() %}
    <h5>the cost of {{ name }} is {{ prise }}. </h5>
    {% endfor %}
    <hr>

    <h3>Conditionals & comparison operators</h3>
    {% for i in numbers%}
    {% if i%2==0 %}
    <h5>even {{i}}. </h5>
    {% endif %}
    {% endfor %}
</main>
{% endblock %}
```

### Wrapping up 

As you can see Jinja provides all those things which you have learned in python.

I highly suggest you have a look through the [Jinja documentation](https://jinja.palletsprojects.com/en/2.11.x/) to grasp even more knowledge of this awesome templating language.

In the upcoming segment we are creating some user interface so the user can interact with the application (e.g., Form, login)


##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help!
[Instagarm](https://www.instagram.com/siddythings/)