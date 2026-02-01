---
layout: post
title: "Dependency Injection in PHP"
date: 2018-04-01
categories: [PHP]
tags: [php]
---

Dependency injection in PHP is a handy technique to keep the code maintainable and re-usable. While coding our PHP classes, we often come to a situation where one class depends on data from another class or a class is dependent on data which is supposed to vary from object to object.


Let's explore a very simple example of How dependency injection helps us write a more clear code.


## Code without dependency injection in PHP

```php
<?php
class User
{
	public $name;
	public $age;

	function __construct($n , $a)
	{
		$this->name = $n;
		$this->age = $a;
	}
}

class Greet
{
	public $User;

	function __construct($n , $a)
	{
		$this->User = new User($n , $a);
		echo "Hello ".$this->User->name."<br/> You are ".$this->User->age." Years Old";
	}
}
$g = new Greet("Zohaib",31);
?>
```

The code above, uses no dependency injection as you can see, Greet class is creating an object of User class inside it's constructor. In such case, Greet class is tightly coupled with User class. Not all, but many changes in User class can cause Greet class to stop working. If we change access modifier of $name property in User class to private, Our code will end up in errors.

## Code with dependency injection in PHP

Let's change this code to have dependency injection.

```php
<?php
class User
{
	public $name;
	public $age;

	function __construct($n , $a)
	{
		$this->name = $n;
		$this->age = $a;
	}
}
class Greet
{
	public function __construct(User $u)
	{
		echo "Hello $u->name<br/> You are $u->age years old";
	}
}

$u = new User("Zohaib",31);
$g = new Greet($u);
?>
```

Instead of initializing User class in Greet class, we are creating an object of User class outside and supplying this object to Greet class constructor. This way, the dependency on User class is being injected in Greet class.


By using dependency injection in PHP, we achieve loose coupling between objects. Our code remains logically divided and maintainable. We can easily run unit tests on each class, which is difficult in tightly coupled objects.


I didn't plan too much for this post, this is just a short post to help you understand basic concept behind dependency injection in PHP. Please share your valuable thoughts on this topic.
```
