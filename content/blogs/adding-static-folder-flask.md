---
author : "Sidhart"
title : "Adding static files Flask | Ep. 04"
# date : "2020-09-04"
description : "In this part will adding static files in flask application"
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
In this segment, we are learning about how to add ```stylesheets```, ```javascript```, and ```images``` to our application.
<!--more-->
Flask provides a few ways to work with static files.

### Creating stylesheets
If you how to work with the stylesheets and how to add in the stylesheets to your HTML pages then its easy for you.

Flask requires a ```static``` folder the same as the ```templates``` folder in the last segment.

#### Creating a static folder

Go inside our ```application``` folder.
- Step 1: Create a static folder.
- Step 2: Create a CSS folder
- Step 3: Create a style.css inside that CSS folder.

Now, Our project structure something that looks like this.
```
. 
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   ├── static
│   │   └── css
│   │       └── style.css
│   ├── templates
│   │   ├── admin
│   │   │   └── dashboard.html
│   │   └── public
│   │       └── index.html
│   └── views.py
├── requirements.txt
└── wsgi.py
```

Open ```style.css``` file

```<root>/application/static/css/style.css```

```html
body {
  background-color: #666666
}
```
Save it.

Adding stylesheets to ```HTML```.

Like typically you provide a relative path to your stylesheet in the ```<head>```

```e.g., <link rel="stylesheet" href="path/to/your/stylesheet.css">```

#### Linking stylesheets in Flask
Flask has a function called url_for which can be used in our HTML to provide a path to any static file we want to fetch.

Like.,

```<link rel="stylesheet" href="{{  url_for('static',filename='css/style.css'}}">```

Notice the double curly braces ```{{ }}```
These double curly braces are part of the Jinja templating engine that Flask uses to render the HTML files.

Don't worry about ```Jinja templating``` you will learn in the upcoming segment in this series.

In this case ```{{ url_for('static', filename='css/style.css') }}``` 
Jinja will replace the path with the CSS file path.
Flask will render the stylesheet we just created at ```static/css/style.css```

Save your HTML file.

#### Javascript Files

Now we are going to create a Javascript folder that stores all the js files.

- Step 1: Create a folder js inside your static folder.
- Step 2: Create a file name app.js inside the js folder.

Now our application structure looks like this
```
. 
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   ├── static
│   │   ├── css
│   │   │   └── style.css
│   │   └── js
│   │       └── app.js
│   ├── templates
│   │   ├── admin
│   │   │   └── dashboard.html
│   │   └── public
│   │       └── index.html
│   └── views.py
├── requirements.txt
└── wsgi.py
```

Open ```app.js``` file


```html
console.log("Hello Flask app from Javascript?");
```

Now link our ```app.js``` to Html file.
Same as we did it in the CSS segment.

Open ```index.html``` file and add the bellow line.

```
<script src="{{ url_for('static', filename='js/app.js') }}"></script>
```

Your ```index.html``` file looks like this

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
  <title>Index</title>
</head>
<body>
  <h1 style="color: blue">Index</h1>
  <p>Hello Flask</p>
  <script src="{{ url_for('static', filename='js/app.js') }}"></script>
</body>
</html>
```

#### Serving Images

Same as in the last segment we need to add a folder for images.
So, let's do it.
- Step 1: Create folder images inside your static folder.
- Step 2: Store some images in it.

Now your folder structure looks like this.

```
.
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   ├── static
│   │   ├── css
│   │   │   └── style.css
│   │   ├── images
│   │   │   └── img.png
│   │   └── js
│   │       └── app.js
│   ├── templates
│   │   ├── admin
│   │   │   └── dashboard.html
│   │   └── public
│   │       └── index.html
│   └── views.py
├── requirements.txt
└── wsgi.py
```

Now add the image to our ```index.html``` file.

Open ```index.html```

as same as we did for CSS and JS we do the same for Images also. we are using url_for for adding the images
Add the following line

```<img src="{{ url_for('static', filename='images/img.png') }}">```

Now our ```index.html``` file will look like this.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
  <title>Index</title>
</head>
<body>
  <h1 style="color: blue">Index</h1>
  <p>Hello Flask</p>
  <img src="{{ url_for('static', filename='images/img.png') }}">
  <script src="{{ url_for('static', filename='js/app.js') }}"></script>
</body>
</html>
```

So Save all the file and Lunch the application

```
flask run
```

### Wrapping Up

In the segment, we have learned a lot of things adding ```CSS```, ```JS```, and ```Images```.
Knowing some ```Jinja``` tools.

Now I think you can make a small static website in Flask. So what you're waiting for go for it. Make your website. 

In the upcoming segments, we are gonna learn about Jinja templating engine and pushing ourselves to communicate with the flask application to HTML views.


##### If you stuck someware checkout my code
[Github](https://github.com/Apex1000/flask-blog)