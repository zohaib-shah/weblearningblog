---
layout: post
title: "How to check if Django is installed on Windows"
date: 2020-06-13
categories: [Django]
tags: [django, python]
---

# How to check if Django is installed on Windows
Django is a popular web framework based on Python. If  you have installed it on your windows machine, either you have done it on core Python installation or on a Python virtual environment.


As Django is a Python web framework, we need to first check if Python is installed. To check this, open command Prompt using `cmd` on Windows + R. Now type `py --version`


After confirming that Python is already setup on our windows PC, we can now check if Django is installed on core Python installation by typing `django-admin --version`


In another case, if Django was installed on a Python virtual environment instead of Python core installation, we first have to go into that virtual environment by using `workon yourvirtualenvname` . Once we are in the specified virtual environment, we can now run `django-admin --version`
