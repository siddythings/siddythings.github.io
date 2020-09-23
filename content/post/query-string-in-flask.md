+++
author = "Sidhart"
title = "Query strings in Flask  | Ep. 12"
date = "2020-09-11"
description = "Creating and working with query string data in Flask"
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

Hello, 

In this segment, we're going to working with query strings. A query string is part of the URL as a string of parameters.
<!--more-->
### 

A query string is used as a string of values sent by the client to the server.

Like..
```
https://www.google.com/search?q=query+string
```

Let's break down the URL:
- ```https``` is the protocol
- ```www.google.com``` is the domain
- ```/search``` is the path
- ```?q=query+string``` is the query string


### Query string

Query string element of the URL, we see the following components:

- ```?``` starts the query string
- ```q``` is the first parameter
- ```=``` separates/assigns a value to the parameter
- ```query+string``` is the value assigned to the q parameter

```Note:-``` You'll notice the + substitution between ```"query string"```. This is because the space characters are not allowed in a URL so must be replaced with something else.

Spaces in query strings are replaced with ```+``` or ```%20```

We use```& ```to separate parameters and values in the query string. Let's create a query string below with a few sets of params & values:

```
http://127.0.0.1:5000/query?foo=foo&bar=bar&baz=baz&title=query+strings+with+flask
```

### Let's create in the application
Flask query strings
First up, let's create a new route with the URL /query:

```views.py```
```py
@app.route("/query")
def query():
    return "No query string received", 200
```

```application/views.py```
```py
from flask import request
```

We use ```request.get_json() ```to serialize incoming JSON data, we use request.args to parse and serialize the query string into a Python object.
Let's store our query string object as a variable called args and print them:

```application/views.py```
```py
@app.route("/query")
def query():

    args = request.args

    print(args)

    return "No query string received", 200
```

You'll see our keys and values printed out to the console:
```
ImmutableMultiDict([('foo', 'foo'), ('bar', 'bar'), ('baz', 'baz'), ('title', 'query strings with flask')])
```
Just like a dictionary, we can now pluck out values by their key:
```py
@app.route("/query")
def query():

    args = request.args

    if "foo" in args:
        foo = args["foo"]

    if "bar" in args:
        bar = args.get("bar")

    if "baz" in args:
        baz = args["baz"]

    if "title" in request.args:
        title = request.args.get("title")

    print(foo, bar, baz, title)

    return "No query string received", 200
```

### Wrapping up 

Query strings are a way to pass arguments to your application and Flask makes light work of quickly parsing them into something we can work with.

##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)