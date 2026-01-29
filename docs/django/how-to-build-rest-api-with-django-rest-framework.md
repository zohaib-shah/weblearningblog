---
layout: post
title: "How to build REST API with Django REST Framework"
date: 2021-01-29
categories: [Django, Python]
tags: [django, python]
---

# How to build REST API with Django REST Framework
Django is a complete web application framework written in Python. We can use it to build any kind of web application. Building REST API with Django REST Framework is required for almost all Django based web applications.


REST based APIs are now a standard way to communicate between two isolated systems.


The trend has shifted towards frontend Javascript frameworks and mobile applications. Extending the capability of the core application is increasing rapidly. This extension should be standard and secure.


As a good practice, IT teams are now planning for the REST API for their software alongside the software itself.


The best way to build a REST API for your Django application is to install Django REST Framework. Django rest framework aka DRF, easily converts Django app into a REST API.


In this post, we will follow below steps to create a simple DRF project :


Setting up virtual environment, Django and Django REST FrameworkConfiguring MySQL database with applicationSetting up Models and RelationsCreating Super User and connecting models with Django admin panelUnderstanding ViewSets and ModelViewSet in DRFWriting ModelViewSet and ModelSerializerAdding REST API routes with DRF SimpleRouter()Using Postman to test our REST APIWorking with OneToMany and ManyToMany in DRFCustomizing response data in Django REST Framework by Overriding ModelSerializer and ModelViewSet


## Setting up virtual environment, Django and Django REST Framework


I assume that you are working on Ubuntu, if not some steps may be different for your OS. Also, make sure that you have Python, PIP and virtualenv installed on your system. You can review some basics here
.


Create a directory where you want your django rest API to live `mkdir drf-crud`. Now `cd` into the directory and run `virtualenv env`


Here, "env" is the name of the environment and you can name it anything as you want. Once the virtual environment is ready, we will activate it with `source env/bin/activate`.


First up, we will install django in our virtual environment with `pip install django`. Now install django rest framework package with `pip install djangorestframework`.


Now create a django project with `django-admin` utility using `django-admin startproject api .` command (The "." at the end of the command is to create the project in the same directory). Use `django-admin startapp app` command to add a new app. The directory structure will look like as below:


Don't forget to add 'rest_framework' and 'app' in INSTALLED_APPS list in `api/settings.py` file.


INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'app'
]


## Configuring MySQL database with application


. Now install python sqlclient package with `pip install mysqlclient`. This package will make sure that our python app is able to communicate with mysql server.


Now replace `DATABASES` dict with following code in your `api/settings.py` file.

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'your_db_name',
        'USER': 'your_db_username',
        'PASSWORD': 'your_db_pw',
        'PORT': '3306'
    }
}


You can now run migrations with following code:


`python3 manage.py makemigrations`
 `python3 manage.py migrate`


## Setting up Models and Relations


We will create three models (tables) .i.e. Product, Category and Tag. Each category will have multiple products (One to Many). Every product can have multiple tags and every tag may also be associated with multiple products (Many to Many). Add following code in `app/models.py` file:


from django.db import models
class Category(models.Model):
	title = models.CharField(max_length=200)
	created_at = models.DateTimeField(auto_now_add=True)
	updated_at = models.DateTimeField(auto_now=True)
	def __str__(self):
		return self.title
class Tag(models.Model):
	title = models.CharField(max_length=200)
	created_at = models.DateTimeField(auto_now_add=True)
	updated_at = models.DateTimeField(auto_now=True)
	def __str__(self):
		return self.title
class Product(models.Model):
	title = models.CharField(max_length=200)
	description = models.TextField()
	category = models.ForeignKey(Category,on_delete=models.DO_NOTHING)
	price = models.DecimalField(max_digits=10,decimal_places=2)
	tags = models.ManyToManyField(Tag)
	created_at = models.DateTimeField(auto_now_add=True)
	updated_at = models.DateTimeField(auto_now=True)
	def __str__(self):
		return self.title


We will now make and run migrations again. This will now create desired tables in the database.


## Creating Super User and connecting models with Django admin panel


Before configuring URLs and ViewSets for our REST API, we should first add some data in our newly added tables. As Django provides us with a convenient admin panel out of the box, we will utilize it by creating super admin user with `python3 manage.py createsuperuser` command.


We need to tell Django admin about what models we would like to be available in the admin panel. Open `app/admin.py` file and add following code:

from django.contrib import admin
from .models import Category
from .models import Product
from .models import Tag
admin.site.register(Category)
admin.site.register(Product)
admin.site.register(Tag)


## Understanding ViewSets and ModelViewSet in DRF


Usually, we would create a function in `views.py` file and configure it in our `urls.py` file so that we could have a working URL route.


ViewSets offer a different approach. ViewSets allow us to create a class based view and utilize it in our URL file. The benefit of this approach is to write minimum code and have the CRUD operations available through a ViewSet class.


Django REST Framework provides many useful ViewSets classes. For the time being, we are only interested in ViewSet and ModelViewSet. We may create an object from ViewSet and override some methods to obtain the CRUD functionality. As the name suggests, ModelViewSet class will take any Django Model and create CRUD features for us without even overriding any of the methods, we will only override methods when we need to write some custom logic.


## Writing ModelViewSet and ModelSerializer


Category model is the simplest of all defined models as it is not depending on either "Product" or "Tag" model.


Let's write our first ModelViewSet for "Category" model. Before we do that, let's first create a serializer for this model. **A ModelSerializer will convert the Django Model to it's JSON representation which is useful for RESTful APIs data exchange. **Create a new file `serializers.py` in `app` directory and write following code:


from rest_framework import serializers
from .models import Category
class CategorySerializer(serializers.ModelSerializer):
	class Meta:
		model = Category
		fields = ('title',)


Now create a `views.py` file as well which should contain below mentioned code:

from django.shortcuts import render
from rest_framework.viewsets import ModelViewSet
from .models import Category
from .serializers import CategorySerializer
# Create your views here.
class CategoryViewSet(ModelViewSet):
	model = Category
	serializer_class = CategorySerializer
	queryset = Category.objects.all()


The CategorySerializer class is being extended from ModelSerializer. ModelSerializer class will take model name to work for any model. We can also set which fields of the model should be available with `fields` property. We can also exclude any field by setting `exclude` property.


The CategoryViewSet class is being inherited from ModelViewSet. We need to override some fields to make it utilize our Category model.


## Adding REST API routes with DRF SimpleRouter()


After defining ModelViewSet and ModelSerializer, we will now add our routes so that we can access our resources. Usually, we should be creating a `urls.py` file in each of our apps. For now, we are adding our routes in the main `urls.py` file.


Add following code into `urls.py` file of your main project (in our case it is named as "api").

from django.contrib import admin
from django.urls import path
from rest_framework import routers
from app.views import CategoryViewSet

r = routers.SimpleRouter()
r.register(r'categories',CategoryViewSet)

urlpatterns = [
    path('admin/', admin.site.urls),
]

urlpatterns += r.urls


As you can see, we are using DRF SimpleRouter() and registering CategoryViewSet as "categories" routes. SimpleRouter() will now automatically generate `list, create, update, retrieve, partial_update` and `destroy` routes. More explanation given in below list:


### GET /categories/ to access all categories


### POST /categories/ to insert a category


### GET /categories/id/ to retrieve specific category


### PUT /categories/id/ to update specific category


**PATCH** /categories/id/ to partial update any category**DELETE** /categories/id/ to delete the specific category


## Working with OneToMany and ManyToMany in DRF


So far, we have worked with category model which is standalone. "Category" model is not dependent either on "Tag" or "Product" models (though it is related to Product model with OneToMany relation).


"Tag" model is also independent. Therefore, we can repeat all above steps to create CRUD functionality for "Tag" model.


"Product" model is directly related with both "Category" and "Tag" models. Let's first update our `app/serializers.py`, `app/views.py` and `api/urls.py` files to look like below:

app/serializers.py
from rest_framework import serializers
from .models import Category
from .models import Tag
from .models import Product
class CategorySerializer(serializers.ModelSerializer):
	class Meta:
		model = Category
		fields = ('title',)
class TagSerializer(serializers.ModelSerializer):
	class Meta:
		model = Tag
		fields = ('title',)
class ProductSerializer(serializers.ModelSerializer):
	class Meta:
		model = Product
		fields = ('title','description','category','price','tags',)

app/views.py
from django.shortcuts import render
from rest_framework.viewsets import ModelViewSet
from .models import Category
from .models import Tag
from .models import Product
from .serializers import CategorySerializer
from .serializers import TagSerializer
from .serializers import ProductSerializer
# Create your views here.
class CategoryViewSet(ModelViewSet):
	model = Category
	serializer_class = CategorySerializer
	queryset = Category.objects.all()
class TagViewSet(ModelViewSet):
	model = Tag
	serializer_class = TagSerializer
	queryset = Tag.objects.all()
class ProductViewSet(ModelViewSet):
	model = Product
	serializer_class = ProductSerializer
	queryset = Product.objects.all()

api/urls.py
from django.contrib import admin
from django.urls import path
from rest_framework import routers
from app.views import CategoryViewSet
from app.views import TagViewSet
from app.views import ProductViewSet

r = routers.SimpleRouter()
r.register(r'categories',CategoryViewSet)
r.register(r'tags',TagViewSet)
r.register(r'products',ProductViewSet)

urlpatterns = [
    path('admin/', admin.site.urls),
]

urlpatterns += r.urls


Important to note here is the presence of 'category' and 'tags' fields in the `ProductSerializer` class. Both these fields are actually a relation with "Category" and "Tags" models. Let's try posting to **/products/** URL to insert a new Product with it's related "Category" and "Tags" fields.


Inserting a record in DRF related models


By default, to add a relation with a record, we have to provide the the *id* of the related model. Similarly, we are saving a product which has category of id 1 and has tags with id 1 and 2 associated with it. It should also be noted that "Product" and "Tag" are in ManyToMany relation, therefore, we have to provide "tags" field as an array of tag ids.


After saving the first Product by **POST**ing JSON to **/products/**. Running a **GET** request to **/products/** returns following data:


We can see that "category" field returned as category *id* and "tags" field returned as an array of tag *ids*. This is the default behavior of ModelViewSet. We have the option to make our ModelViewSet return more meaningful and useful data instead of *ids* for the relational fields.


## Customizing response data in Django REST Framework by Overriding ModelSerializer and ModelViewSet


We are using ModelViewSet and ModelSerializer to quickly achieve CRUD functionality for our API. Both these classes offer us to customize the behavior of almost any feature by overriding pre-defined methods.


Going deep into customization of ModelViewSet and ModelSerializer is out of scope of this post. In our case, we will attempt to return "Tag" title instead of *ids* whenever a product is fetched.


from rest_framework import serializers
from .models import Category
from .models import Tag
from .models import Product
class CategorySerializer(serializers.ModelSerializer):
	class Meta:
		model = Category
		fields = ('title',)
class TagSerializer(serializers.ModelSerializer):
	class Meta:
		model = Tag
		fields = ('title',)
class ProductSerializer(serializers.ModelSerializer):
	class Meta:
		model = Product
		fields = ('title','description','category','price','tags',)
	def to_representation(self, obj):
		data = super().to_representation(obj)
		#data is of type orderedDict
		#orderedDict is similar to Dict data type with an exception of preserving order
		#we can customize data from this point as per our needs
		tags_info = Tag.objects.filter(pk__in=data['tags'])
		cat_name = Category.objects.get(pk=data['category'])
		new_tags_fields = []
		for ti in tags_info:
			new_tags_fields.append(ti.title)
		data['tags'] = new_tags_fields
		data['category'] = cat_name.title
		#once done we have to return the data variable (orderedDict)
		return data


Above code is the modified version of our `app/serializers.py` file. We have overridden the .to_representation() method. Rest of the code is self explanatory.


Following will be the result of above changes:


Now, we are having category name and tag names instead of IDs, which is more meaningful.


## Conclusion


We have covered enough to get started with Django REST Framework in this post. Going forward, we can add authentication and more customization. I will try to cover it in my upcoming posts, Don't forget to subscribe and share this post. Complete code is available at [https://github.com/zohaib-shah/django-rest-api](https://github.com/zohaib-shah/django-rest-api" target="_blank" rel="noreferrer noopener nofollow)
