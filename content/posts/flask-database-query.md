---
author : "Sidhart"
title : "Flask Database Query  | Ep. 11"
# date : "2020-09-10"
description : "Connect Flask to a Database with Flask-SQLAlchemy and Database Query"
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

Hi,
In this segment, we are learning about displaying the records and Querying our data.
<!--more-->
### Inserting Todo from the Form.

We have created a form in an earlier segment, so let's add todos from that form.

### Update the todo route.
```<root>/application/views.py```

```py
@app.route("/todo", methods=["GET","POST"])
def todo():
    if (request.method == "POST"):
      value = request.form["task"]
      new_todo = Todos(todo = value)
      db.session.add(new_todo)  # Adds new Todo record to database
      db.session.commit()
      return render_template("public/main/todo.html",tasks=tasks)
    return render_template("public/main/todo.html")
```

After this update, we can save data in the ```DB``` with our form.

### Showing the records.
For displaying the record we need to trigger a query.

This query fetches all the records from the table.

```<tablename>.query.all()```
Update your ```views.py```
```py
@app.route("/todo", methods=["GET","POST"])
def todo():
    if (request.method == "POST"):
      value = request.form["task"]
      new_todo = Todos(todo = value)
      db.session.add(new_todo)  # Adds new Todo record to database
      db.session.commit()
      return redirect('todo')
    todos = Todos.query.all() # To render all the todo 
    return render_template("public/main/todo.html",todos=todos)
```

Update your ```todo.html ```
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
  {% for i in todos%}
  <ul>
    <li>{{i.todo}}</li> 
  </ul>
  {% endfor %}
</main>
{% endblock %}
```

Notice :- ```{{i.todo}}``` ```SQL``` return dist so ytodoou need to get the value by name.

### Querying a single todos
For selecting a single a record. We are using our id column we created in our ```todos model```

For that we are updating the todos route we create
```py
@app.route('/todo/<task>')
def todos(task):
  todos = Todos.query.get(task) # To render the todo  
  return "<p> Your task "+task+" is "+todos.todo+"</p>"
```

#### For more understanding of DB try to create a complete Todo application.
with these requirements
- Todo name
- Todo description
- Todo date
- Todo Time
- Complete / Incomplete


### Wrapping up 

In this segment we have learn about query database.
For more info 
Try to do some project by yourself. [flask-sqlalchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)



##### If you stuck someware checkout my code 

[Github](https://github.com/Apex1000/flask-blog)

##### Still need any help! Just DM 
[Instagarm](https://www.instagram.com/siddythings/)