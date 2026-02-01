---
layout: post
title: "Learn OOP in PHP"
date: 2018-02-22
categories: [PHP]
tags: [php]
---

If you want to learn OOP in PHP then it is a good step towards making yourself an expert PHP developer. In this post, I will be focusing on basic concepts of OOP in PHP with a little bit of code.


## What is OOP ?


Object oriented programming is a useful programming technique. In object oriented programming, the code is actually divided into meaningful logical blocks called objects.


## Why we need to Learn OOP in PHP ?

In PHP, or in any other programming language if we keep writing the code without considering the future needs, we may get stuck at some point where application gets bigger. So the solution to this problem is keeping the code modular. Additionally OOP provides us with a concept of centralization of code which eliminates duplication and make the code manageable at one place. Re-usability is another outcome of OOP, which enables us to write once and use many times.


## What is a Class ?


A class is heart of object oriented programming. A class is basically a blueprint or a recipe which can be used to produce many instances of a class .i.e objects. Variables defined inside a class are considered as "Properties of a class", while functions specified within a class is referred to as "Methods of a class". Following is the syntax of a very basic class definition.


```php
class Book
{
public $title;
public $price;

public function __construct($t = "",$p = 0)
{
 $this->title = $t;
 $this->price = $p;
}
}


In this code, we are creating a Book class with two properties .i.e. $title and $price. We are also defining a constructor method which is taking two parameters and assigning them to both properties $title and $price respectively.


## Object in OOP


An object is an instance of a class, different objects of same class may possess different characteristics, they may contain different values for a property. Given below is the code to create two objects of "Book" class.


$mybook = new Book("My PHP Book",49.5);
$yourbook = new Book("Your PHP Book",60);
echo $mybook->title;//echo "My PHP Book"
echo "
";
echo $yourbook->title;//echo "Your PHP Book"


We create an object by using new keyword of PHP. Notice that we are supplying two parameters while creating the object, first is a string for book title and second is double for book price. The order of these parameters and their data type should be same as defined in constructor of the class.


## What is Encapsulation ?


While learning OOP in PHP, Concept of encapsulation is very handy. Sometimes it becomes necessary to keep control of a property or a method of a class limited to only that class while on the other hand, it is alright to extend this control to the inherited class as well and mostly we can expose this control to outside world .i.e. All the objects of that class could set the value of the property directly as seen in the above code $mybook->title is setting the value of property $title of the class by accessing it using ->operator.


Encapsulation is generally considered data hiding. It is hiding of data (properties and methods of a class) from classes to classes and objects to classes. Encapsulation is achieved by using access modifiers


## Class Inheritance


A class can be extended from another class and act as a child to that class. Child classes may contain additional properties and methods while it must be carrying all the properties and methods of parent class.


If we have one parent class "Product" and two sub-classes "Laptop" and "Shirt", Take a look at the code below:


class Product
{
	public $title = "";
	public $price = 0.00;

	public function __construct($t,$p)
	{
		$this->title = $t;
		$this->price = (float)$p;

	}
}

class Shirt extends Product
{

}

class Laptop extends Product
{

}
$s = new Shirt("Formal Shirt",100);
$l = new Laptop("Dell Laptop",99);
echo "Shirt title is ".$s->title;//echi Shirt title is Formal Shirt
echo "
";
echo "Laptop title is ".$l->title; //echo Laptop title is Dell Laptop


We are creating two objects, one from "Shirt" class and other from "Laptop" class. We know that Laptop and Shirt are two distinct entities, but we can classify them as "Product", therefore we created a parent class "Product" and add two properties to that class .i.e. "title" and "price".


Practically, we create a common parent class when we feel that two classes are partially similar to each other, we think about creating a common class in which all the common properties and methods are kept. As in above example code, "title" and "price" are common attributes of both Laptop and Shirt products.


## Conclusion


This is just a short introduction to make it easy for you to learn OOP in PHP. It is a broad topic; Hence covering it in one post is impossible. I have tried to cover most common concepts here, but each of the sub topics discussed above, deserves a dedicated post.


Shortly, I will be posting more detailed and code oriented articles, so that you can learn OOP in PHP in a more clear way.

```