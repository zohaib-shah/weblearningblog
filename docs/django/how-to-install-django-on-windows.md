---
layout: post
title: "How to Install Django on Windows"
date: 2020-08-16
categories: [Django, Python]
tags: [django, python]
---

# How to Install Django on Windows
Installing Django on windows is a fairly simple process. Django is web framework which uses Python as it's programming language.


The first step is to check if Python is installed on Windows, Check it with following command:


`py --version`


If Python is not installed, Please go to [https://www.python.org/downloads/](https://www.python.org/downloads/) and install it as you do it for any other windows software.


## Python Virtual Environment


Python Virtual Environment is handy when we want to isolate different projects and their dependencies.


As with other programming languages, Python also have tons of libraries and third party extensions (In fact Python is leading this race). Virtual environment is useful to avoid conflicts between these packages being utilized in multiple projects on a single Python installation.


To install virtual environment, run following commands:


`pip install virtualenv`
`pip install virtualenvwrapper-win`


Double check with following command:


`virtualenv --version`


## Creating Virtual Environment for Django


Now that we have setup virtualenv for python, we will create virtual environment to house our Django project, run the following command:


`mkvirtualenv django-project`. Now run `workon django-project` to get into the created virtual environment. To check which packages are installed, we can run `pip freeze`. Important to note here is that `pip freeze` will show packages within a virtual environment only if we are inside that virtualenv, otherwise it will show all the packages installed globally over the python installation.


## Install Django inside the Virtual Environment


While we are still in the created virtual env, we will run `pip install Django`. This will download and install Django for us.


Once Django is installed, `django-admin` utility will be available for us and we can make use of various different commands to work with our Django project using this.


For now, run `django-admin --version` to check the version of the Django framework.


## Creating your first Django Project


To create a django project, we will use `django-admin startproject your_project_name`. You can now navigate to the directory which is currently active in your command prompt. You should be able to see a new directory created with the name you have given to your project.


This is the basic structure of a djang app. Now `cd your_django_project` and run `py manage.py runserver`


You now have your django app running on localhost, browse http://127.0.0.1:8000, it should load the following default welcome page:


Django Welcome Page


Share your thoughts and Keep following my blog, I will be posting more content on Django Development.
