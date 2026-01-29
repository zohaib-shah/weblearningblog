---
layout: post
title: "Important String Functions in PHP"
date: 2018-01-13
categories: [PHP, Programming]
tags: [php, string-functions, web-development]
---

# Important String Functions in PHP

Some functions are very common and we use them frequently in our projects. No matter which CMS or Framework we are using but as long as they are based on PHP we can utilize these PHP functions to perform some basic tasks.

## implode() in PHP

The `implode()` function converts an array into a string and separates each element with the separator string provided as second parameter.

```php
<?php
$fruits = array('Mango', 'Oranges', 'Banana', 'Apple');
echo implode(", ", $fruits);
// Output: Mango, Oranges, Banana, Apple
?>
```

## explode() in PHP

PHP `explode()` function breaks a string based on the delimiter and returns an array.

### Parameters

- **Delimiter** (required) - This is the separator string
- **String** (required) - This is the string which will be converted into array
- **Limit** (optional) - If provided it will limit the elements of the array and last element will contain rest of the string

```php
<?php
$fruits = 'Mango,Oranges,Banana,Apple';
$arr = explode(",", $fruits);
print_r($arr);
// Array ( [0] => Mango [1] => Oranges [2] => Banana [3] => Apple )
?>
```

Following code uses limit parameter as well:

```php
<?php
$fruits = 'Mango,Oranges,Banana,Apple';
$arr = explode(",", $fruits, 2);
print_r($arr);
// Array ( [0] => Mango [1] => Oranges,Banana,Apple )
?>
```

## lcfirst() in PHP

The `lcfirst()` function in PHP makes first character of the string lowercase.

### Parameters

- **string** - The string

### Returns

- **string** - returned string with first character lowercased

```php
<?php
$str = "Hello World";
echo lcfirst($str);
// Output: hello World
?>
```

## str_replace() in PHP

The `str_replace()` function replaces the searched string in a string. Search string and replace string is passed as first and second argument while the third argument is string itself.

We can supply string or array as arguments for search and replace. Let's have a look at how the function performs in both situations.

```php
<?php
$str = "Hello World";
echo str_replace("World", "PHP", $str);
// Output: Hello PHP
?>
```

## strlen() in PHP

The `strlen()` function returns the number of characters in a string. This function is useful in validations and mostly used in string manipulations in combination with other string functions of PHP.

```php
<?php
$str = "Hello World";
echo strlen($str);
// Output: 11
?>
```

`strlen()` function takes string as argument and returns integer. `strlen()` also counts spaces.
