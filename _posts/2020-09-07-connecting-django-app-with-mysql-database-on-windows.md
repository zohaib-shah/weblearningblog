---
layout: post
title: "Connecting Django App with MySql database on Windows"
date: 2020-09-07
categories: [Django]
tags: [django, python, mysql, database]
---

Connecting Django app with MySQL database on Windows is the basic step in building a MySQL based website in Django. In this post, we will see how we can connect mysql database with Django.


I assume that you have already Django installed inside the Python Virtual environment. If not, you can review [this](http://weblearningblog.com/django/how-to-install-django-on-windows/" target="_blank" rel="noopener noreferrer) link.


## Creating the Django Project


Before we connect mysql database with django app, we need to create a new django project using the django-admin utility.


Open the windows command prompt and type `workon django-project` to enter your python virtual environment. Now run `django-admin startproject hrm`. Here, "hrm" is the project name and could be anything of your choice. Now cd into your project folder and run `py manage.py runserver`. You should see something like below:


Django Server starting


If you focus carefully on above image, it is saying that the django built-in server (good enough for testing and development phase) has started on http://127.0.0.1:8000. If you browse the same URL, you should see django welcome page. Also, in the same image you can see that we have been informed about the pending migrations.


All modern web development frameworks support migrations. With migrations, we can avoid creating tables and schema using mysql commands. Instead, we can define the table structures and relations using the programming language syntax. In short, Migration migrates the structure and relations written in programming language syntax into the database.


## Configuring MySQL database with Django


In order to connect mysql database with django app,We need to install MySQL python client into virtual environment. Run `pip install mysqlclient`. If the server is still running, you need to `Ctrl + C` before installing any python package.


Chances are high that `pip install mysqlclient` results in error on windows. If this is the case with you as well, you should consider installing it through a wheel. Some useful wheels are found [here](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient" target="_blank" rel="noopener noreferrer). Once you have downloaded the required wheel file, just run following command while still in the virtual environment:
pip install D:\downloads\mysqlclient-1.4.6-c
p38-cp38-win32.whl


To verify the installation of the mysqlclient package, run `pip freeze`. You should see a list of packages installed within your virtual environment and mysqlclient should be one of them.


 While SQLite is default, Django officially [supports](https://docs.djangoproject.com/en/3.1/ref/databases/" target="_blank" rel="noopener noreferrer) multiple databases out of the box. I assume that you have XAMPP setup on your PC. if not, you can use any method to create mysql database for this tutorial.


We have the project setup now. We now have to configure MySQL with django application using the settings.py file.


Find and comment the below code block in your settings.py file:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Now add the below code block to tell Django that you will indeed use MySQL and not SQLite.

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'your_database_name',
        'USER': 'your_database_user',
        'PASSWORD': 'your_database_password',
        'PORT': '3306',
        'HOST': 'your_database_host'
    }
}
```

At this point, we have installed mysqlclient and configured the database information in settings.py file. We will now run the migration, which will create and populate all the tables in the database automatically. Following two commands will run the migration:

```bash
py manage.py makemigrations
py manage.py migrate
```

It should be noted here that we should always be inside the project folder (in command prompt) to run any of the manage.py commands.


This is all we need to connect mysql with django app. Share your thoughts in the comment section below.
