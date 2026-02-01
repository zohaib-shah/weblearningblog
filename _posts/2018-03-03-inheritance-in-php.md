---
layout: post
title: "Inheritance in PHP"
date: 2018-03-03
categories: [PHP]
tags: [php]
---

Inheritance in PHP is useful when we need to create classes with some common, while few contrasting behaviors. In such case, we create a parent class for shared characteristics and child classes for distinct ones.

We discussed inheritance in our ["Learn OOP in PHP"](http://weblearningblog.com/php/learn-oop-php/) post, we should extend our code from example given there.

## Setting Up Classes

For better understanding of inheritance in PHP, consider a scenario we might face while developing an E-Commerce website, We may be having products of different categories. Products related to distinct categories may contain attributes which could be identical to a product of other category. For example, a "shirt" in "Clothes" category would have a "title" attribute and a "laptop" in "Electronics" category would also have "title" property.
```php
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

In the code above we are defining a parent class "Product" and extending two child classes named "shirt" and "laptop".

## Overriding properties and methods of parent class

In general, A property or method of a parent class with "public" or "protected" access modifiers is accessible from child class. If we re-declare a property or method of a parent class in child class, it is considered as Overridden.
class Product
{
	public $title = "Product Title";
	public $price = 0.00;
        public function __construct()
	{
		echo $this->title;
        }
}

class Shirt extends Product
{
    public $title = "Shirt Title";
}
$s = new Shirt();

In above code, we are re-declaring the property "$title" in Shirt class. The constructor of the parent class is echoing the $title property. Upon executing this code, you can see that it prints "Shirt Title", which is defined in child class. It concludes that if an object of child class accesses a property which is overridden in child class than the return value would be same as defined in that child class.

Also notice that we have not defined any constructor in child class "Shirt". This is the reason why constructor of parent class "Product" is called whenever any object is created from "Shirt" class.

Just like properties, we may also override methods in child classes. This is useful if we want objects of child class behave differently in some cases. Following code demonstrates the use of method overriding in PHP:
title;
	}

	public function summarize()
	{
		echo "Product Price is: $this->price and Title is: $this->title";
	}
}
class Shirt extends Product
{
  public $title = "Shirt Title";
  public function summarize()
	{
		echo "Shirt Price is: $this->price and Title is: $this->title";
	}
}
$s = new Shirt();
$s->summarize();

The method "summarize()" has been defined in parent class as well as in child class, now whenever an object of child class calls "summarize()", the method defined in child class will be executed.

## Using abstract classes to implement inheritance in PHP

Abstract classes are different from normal classes. It is not allowed to create objects directly from abstract classes. Abstract classes are incomplete in nature and they are dependent on child classes to complete their functionality.

Abstract classes exist to behave as a parent. Let's observe their behavior by looking at the code below:
title and Price is $this->price";
	}
}
$p = new Product();
?>

Executing above code results in a fatal error which says "Fatal error: Cannot instantiate abstract class Product". We have to create a child class which should extend this abstract class "Product", doing so, we may utilize the functionality of abstract class. Following code executes without any error:
title and Price is $this->price";
	}
}
class Shirt extends Product
{
}
$p = new Shirt();
?>


## Conclusion

In this post, we tried to experiment with the concept of inheritance in PHP. We also tried to keep our approach simple and learn this concept with as little code as possible.
```
