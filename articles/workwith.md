# Work With Interface

* [Introduction](#introduction)
* [Our Solution](#our-solution)
* [Installation](#installation)
* [CSS Frameworks](#css-frameworks)
* [Create Application from _scaffold](#create-application-from-_scaffold)
* [Define the Model](#define-the-model)
* [Define Main Template](#define-main-template)
* [Create grid template](#create-grid-template)
* [Create controller method](#create-controller-method)
* [Wrap Up](#wrap-up)

## Introduction

The hallmark of RPG development on IBM i and traditional AS/400 applications is the `Work With` program built using
subfiles. In my experience as an RPG developer this is a time-consuming process with a great deal of boilerplate
code that must appear in each Work With program. In addition to Work With display, the program typically includes
CRUD capability to create, read, update and delete records.

This article presents an alternative way to build Work With programs that utilizes Python, is fast to develop and
minimizes boilerplate code.

## Our Solution

Our solution utilizes [Python](https://www.python.org) and the [py4web](https://py4web.com) rapid web development
framework. Some of you may
already be familiar
with the [web2py](https://web2py.com) web development framework. py4web is the next generation framework from
Massimo DiPierro, the creator of the
web2py framework.

For our tutorial we'll create a simple contact database with a first name, last name, email address, phone number
and notes. We'll store our data in a SQLite database. SQLite is a tiny relational database included when you
install Python.

## Installation

For this tutorial we'll be installing Python on our local workstations (a future 
article will cover installation/deployment on IBM i).  A complete guide to installation is outside the scope of this 
article.

To get started you'll need both Python and py4web installed on your system.

* [Python](https://www.python.org/downloads/)
* [py4web](https://py4web.com/_documentation/static/en/chapter-03.html#)

Follow the instructions in the links above. Be sure to verify your installation of both Python and py4web before continuing.

## CSS Frameworks

If you're new to web development you may not be aware of CSS frameworks. CSS frameworks provide an easy way get a
consistent look and feel and color scheme applied to your website. There are a number of very good CSS frameworks
out there to choose from. For this tutorial we'll be using the [Bulma](https://bulma.io) framework. Refer to the
[Getting Started with Bulma](https://bulma.io/documentation/overview/start/) page to view the starter template we'll
be using. (don't stress over it, we'll cover it later)

## Create application from _scaffold

At this point you should have

- Python running
- py4web running and have access to the [_dashboard](http://127.0.0.1:8000/_dashboard) app
- Know where your py4web `apps` directory is

Locate the _scaffold directory inside your `apps` directory and copy the entire directory to a new directory. Call the
new directory `contacts`.

In your browser you should now be able to navigate to http://127.0.0.1:8000/contacts and see the default py4web
application page.

## Define the model

Database definitions in py4web live in the `models.py` file in the `contacts` directory. Our application requires only 1
table, the `contact` table.

Open the models.py file and edit it to look like the following.

```python
from .common import db, Field
from pydal.validators import *

db.define_table('contact',
                Field('first_name', length=25, requires=IS_NOT_EMPTY()),
                Field('last_name', length=25, requires=IS_NOT_EMPTY()),
                Field('email', length=256, requires=IS_EMAIL()),
                Field('phone', length=25))
db.commit()
```

The file is fairly self-explanatory.

* Specify our imports
* Define our table
* First name is required
* Last name is required
* Email must be a valid email address
* Phone number is an optional field
* The db.commit() process (prior to the normal database commit) will perform migration on the database. In short,
  this will ensure that the database is set up as defined in the models file.

After saving and restarting py4web you'll now see some changes in `contacts/databases`. storage.db is the SQLite
database used for this application. sql.log is a log of all the SQL
statements run
on the database to create the tables. The other files are the files used to keep track of the status of all
tables/migrations.

## Define Main Template

As mentioned above our application is going to use the Bulma templating system to style our pages. In py4web as
with other web frameworks you can define a master template which all other templates can extend to enforce a
consistent look to your website. In py4web, this master template is commonly named layout.html and is stored in
the `templates` directory.

Open up `templates/layout.html` and copy the following code block into it.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>py4web Contacts</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.14.0/css/all.min.css"
          integrity="sha512-1PKOgIY59xJ8Co8+NE6FZ+LOAZKjy+KY8iq0G4B3CyeY6wYHN3yt9PW0XpSriVlkMXe40PTKnXrLnZ9+fkDaog=="
          crossorigin="anonymous"/>
    <style>.py4web-validation-error {
        margin-top: -16px;
        font-size: 0.8em;
        color: red
    }</style>
    [[block page_head]]<!-- individual pages can customize header here --> [[end]]
</head>
<body>
<section class="section">
    <div class="container">
        [[include]]
    </div>
</section>
<script src="js/utils.js"></script>
[[block page_scripts]]<!-- individual pages can add scripts here --> [[end]]
</body>
</html>
```

This will set our web app to use the bulma CSS framework along with a couple customizations for py4web.

## Create Grid Template

Next we'll create the template to use for our Work With page. Create file `templates/grid.html` and copy in the
following code block.

```html
[[extend 'layout.html']]
[[=grid.render() ]]
```

The first line tell us to extend the layout.html file we created above. This code will be placed in the [[include]]
block in layout.html.

## Create Controller Method

Now that we have all the setup work done we can write the code that will handle our web requests. By default,
controller methods are stored in `controllers.py`. Let's open up `controllers.py` and copy the following imports at
the top of the file.

```python
from py4web.utils.form import FormStyleBulma
from py4web.utils.grid import Grid, GridClassStyleBulma
```

Here we're giving ourselves access to the py4web utilities to build Forms and Grids. Also importing the
Bulma-specific styling methods.

Next, copy the following into the bottom of the file.

```python
@action("contacts", method=["POST", "GET"])
@action("contacts/<path:path>", method=["POST", "GET"])
@action.uses(
    "grid.html",
    session,
    db)
def contacts(path=None):
    grid = Grid(
        path,
        db.contact,
        orderby=[db.contact.last_name, db.contact.first_name],
        formstyle=FormStyleBulma,
        grid_class_style=GridClassStyleBulma
    )

    return dict(grid=grid)

```

Let's do a quick review of what each line above does.

* @action() - these two lines define the url path within the application. The first one handles the grid or select
  page and the second one handles the action (add/change/delete) pages. You see that we accept GET or POST requests
  for each.
* @action.uses() - this line tells us the following
    * the template we'll use for displaying this page
    * additional `fixtures` that are used inside the method. For instance, we need access to the database so we
      specify the db fixture. Session is also needed within the grid so we specify it here. These fixtures are similar
      to decorators that run before and after the controller method runs.
* Grid() - this creates a grid over the contacts table, sorted by last, first name that uses the Bulma CSS
  framework
  for styling.
* return - pass a dict to the template making the `grid` variable available for use in the template.

That's it! Restart the py4web server and then navigate to `http://127.0.0.1:8000/contacts/contacts`.

## Wrap Up

You should now have a fully functional Work With page to add/change/delete contacts in your contact table.

Since this is an introduction article we've skipped over a lot of the details to highlight how simple it is to
create the Work With page. You can refer
to the grid chapter in the
[py4web Manual](https://py4web.com/_documentation/static/en/chapter-14.html) to get a feel for all the other options
available to grid. Additionally there is a github repo with many beginner to advanced tutorials on using the py4web 
grid available [here](https://github.com/jpsteil/grid_tutorial).

Happy Coding!
