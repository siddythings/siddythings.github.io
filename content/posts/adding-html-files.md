---
author : "Sidhart"
title : "Adding HTML files | Ep. 03"
# date : "2020-09-04"
description : "In this part will take care of your HTML files"
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

Hello Guys
in this part, we are going to bind our application with an HTML page.
<!--more-->

Flask provides very easy way work with static files(e.g HTML,CSS,JS).

### Application Setup

Just open the same folder we created in the last series. Your application looks something like this.
```
.
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   └── views.py
├── requirements.txt
└── wsgi.py
```

#### Templates Folder

Flask provides an easy way to store all your HTML files in a single place.

- Step 1: Create a ```templates``` folder inside your application folder.
- Step 2: Create an ```index.html``` file inside that ```templates folder```.

Open ```index.html``` file
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Index</title>
</head>
<body>
  <h1 style="color: blue">Index</h1>
  <p>Hello world from HTML file.</p>
</body>
</html>
```

Now your project structure looks like this.
```
.
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   ├── templates
│   │   └── index.html
│   └── views.py
├── requirements.txt
└── wsgi.py
```
Binding templates to views.

Open ```views.py``` file
```python
from flask import render_template
from application import app
@app.route('/')
def index():
    return render_template("index.html")
```

You've noticed in this file we added two lines 
```from flask import render_template``` is used to importing ```render_template``` which help's in rendering the HTML file.

And ```return render_template("index.html")``` this will look in the templates folder and render the HTML file.

Save it.!

### Lunch application

Hit the following command from your parent folder.

```flask run ```

Go to [http://127.0.0.1:5000/](http://127.0.0.1:5000/)

Now you'll see your HTML file.


### Leveling Up.

- Create HTML files for about and blog routes.
- Create HTML files for admin dashboard routes.

```Tip:-``` Always make a separate folder every new feature, This will help to make your project structure clean.

After this, your project structure looks like this.
```
.
├── application
│   ├── __init__.py
│   ├── admin_views.py
│   ├── templates
│   │   ├── admin
│   │   │   └── dashboard.html
│   │   └── public
│   │       └── index.html
│   └── views.py
├── requirements.txt
└── wsgi.py
```


While rendering HTML file just add the folder name.
```eg.  return render_template("public/index.html")```

### Wrapping up
Any application can be easy as well as complex.

You can simply put all your views in a single folder and you can also place in a different folder is up to you.

In the next series, we are going to look at static files (e.g., CSS, JS, Images).


##### If you stuck someware checkout my code
[Github](https://github.com/Apex1000/flask-blog)