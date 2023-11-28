---
author : "Sidhart"
title : "Flask configuration files | Ep. 09"
# date : "2020-09-08"
description : "Configuring Flask applications using a config file"
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

In this segment, we are talking about the configuration file. We're going to talk about the configuration method, using a config file.
<!--more-->

The configuration is an important part of any application and, Flask provides many different ways to configure your application like setting your environment type and depending upon there database.

### Config basics

Every Flask application has a global config object which can be accessed via ```app.config```.

```config object``` allows us to assign values to configuration variables, which can be used in the application.

You can see your ```Global config objects``` just simply print it one of your routes. Go ahead and add the following line.

```print(app.config)```

And you will see this in your terminal.
```
{
	'ENV': 'development',
	'DEBUG': False,
	'TESTING': False,
	'PROPAGATE_EXCEPTIONS': None,
	'PRESERVE_CONTEXT_ON_EXCEPTION': None,
	'SECRET_KEY': None,
	'PERMANENT_SESSION_LIFETIME': datetime.timedelta(31),
	'USE_X_SENDFILE': False,
	'SERVER_NAME': None,
	'APPLICATION_ROOT': '/',
	'SESSION_COOKIE_NAME': 'session',
	'SESSION_COOKIE_DOMAIN': None,
	'SESSION_COOKIE_PATH': None,
	'SESSION_COOKIE_HTTPONLY': True,
	'SESSION_COOKIE_SECURE': False,
	'SESSION_COOKIE_SAMESITE': None,
	'SESSION_REFRESH_EACH_REQUEST': True,
	'MAX_CONTENT_LENGTH': None,
	'SEND_FILE_MAX_AGE_DEFAULT': datetime.timedelta(0, 43200),
	'TRAP_BAD_REQUEST_ERRORS': None,
	'TRAP_HTTP_EXCEPTIONS': False,
	'EXPLAIN_TEMPLATE_LOADING': False,
	'PREFERRED_URL_SCHEME': 'http',
	'JSON_AS_ASCII': True,
	'JSON_SORT_KEYS': True,
	'JSONIFY_PRETTYPRINT_REGULAR': False,
	'JSONIFY_MIMETYPE': 'application/json',
	'TEMPLATES_AUTO_RELOAD': None,
	'MAX_COOKIE_SIZE': 4093,
}
```

All these are the default ```built-in configuration```variables, which we can modify and change.

To change any of these variable, we just need to assign a new value to the key:
```app.config["KEY"] = "value```

For example, let's update the configuration ```SECRET_KEY``` variable with the following:

```app.config["SECRET_KEY"] = "qwert653443"```

```print(app.config["SECRET_KEY"])```

You'll see the following in your terminal:
```
qwert653443
```
Let us define a config file for our application.

### App config file

Flask allows us to create a custom ```config``` file. 
so it can overwrite the default ```config``` variables.

So let's create of ```config.py``` file in the root folder.

```<root>/config.py```
```py
class Config(object):
    DEBUG = False
    TESTING = False

class Production(Config):
    pass

class Development(Config):
    DEBUG = True
```
We first create the Config class and set some default attributes.

We have created 3 more classes and each of which inherits the Config class and attributes.

- Production - Is the config class we'll use for running in production
- Development - Is the config class we'll use for development


Let's create our config file 
```py
class Config:
    """Base config."""
    SECRET_KEY = "worldisbest"
    STATIC_FOLDER = 'static'
    TEMPLATES_FOLDER = 'templates'
   
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
We've assigned new values 

Now that we've created our config file, we'll need to instruct Flask to load it.

### Loading a config file
Adding a config file is a simple one-liner and should be placed as close to wherever you've created your app object.

The best way to add a config file is just after you create your app object.

We load a config file with ```from_object()``` method
```e.g. app.config.from_object"config_filename.ConfigClass")```

If you want to load the Development class, we would do the following.
```app.config.from_object("config.Development")```

If you want to load the Production class, we would do the following.
```app.config.from_object("config.Production")```

And after all, you open your ```__init__.py``` file 
add your config statement just bellow your ```app = Flask(__name__)``` line
```py
app = Flask(__name__)
app.config.from_object("config.Development")
```

We can also use the ```ENV variable``` to set the condition in which the config file will load.

### ENV environment

The ```ENV environment configuration``` variable is important and should always be set outside of your application, which we set with FLASK_ENV from the terminal.

Like in the first segment of the series

By default , ```ENV``` set to production which disables the ```DEBUG``` mode and avoids displaying the interactive debugger

Setting ```FLASK_ENV=development``` enables the debugger, which should never, ever be done in production!

Now when we set the ENV variable so we can so with the following:
```py
app = Flask(__name__)

if app.config["ENV"] == "production":
    app.config.from_object("config.Production")
else:
    app.config.from_object("config.Development")
```
This allows us to control the config environment outside the application by setting the FLASK_ENV environment variable in the terminal.

IF you set ```FLASK_ENV to development```
```shell
$ export FLASK_ENV=development
$ flask run
* Environment: development
 * Debug mode: on
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 249-444-542
```
Now repeat the process, changing ```FLASK_ENV to production:```
```
$ export FLASK_ENV=production
$ flask run
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
```

### Wrapping up 

In this segment we have learn about seting up the config file.

Try to do some project by yourself.

In the upcoming segment we are learing about linking Database to our application.


##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)