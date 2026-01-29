---
layout: post
title: "Understanding null coalescing and ternary operator with truthy and falsy values in Javascript"
date: 2024-01-25
categories: [JavaScript]
tags: [javascript]
---

# Understanding null coalescing and ternary operator with truthy and falsy values in Javascript
Understanding how javascript evaluates different values as truthy or falsy is an essential need in day to day coding. Truthy and falsy values in javascript are used in if blocks, ternary and OR (||) operators and null coalescing (??) operator.


To understand truthy and falsy values in javascript, we will declare multiple variables of different types.


```javascript
const nonEmptyString = "Anything";
const emptyString = "";

const numericZero = 0;
const numericNonZero = 5;

const boolFalse = false;
const stringFalse = "false";

const emptyArray = [];
const nonEmptyArray = ['an element'];


To check for exact match, we usually write if statement as below:


if(nonEmptyString === "Anything"){
console.log("Matched");
}

// outputs "Matched"


Truthy and falsy way of assessment is useful when we are not interested in exact comparison but only want to check for either the criteria is generally acceptable. Following are some examples of what we mean here by "Generally acceptable":


When we only want to check if an input field is filled instead of what exactly it contains.


When we want to make sure that an array has elements but we are not keen to know how many elements or what elements are in it.


When we want to ensure that a variable with number type has non zero value .i.e. anything but zero.


To examine any value for truthiness, we use if statement as below:


if(nonEmptyString){
    console.log("Truthy");
} else {
    console.log("Falsy");
}


and following is the result of above variables when we test them for truthiness:


const nonEmptyString = "Anything"; // true
const emptyString = ""; // false

const numericZero = 0; // false
const numericNonZero = 5; // true

const boolFalse = false; // false
const stringFalse = "false"; // true

const emptyArray = []; // true
const nonEmptyArray = ['an element']; // true


Surprisingly, an empty array yields as truth. So, how can we check if an array is filled or empty using truthy and falsy way of assessment?


We do check an array with its length property. If an array is empty, its length is zero and from the above results we can see that numeric zero is false. Any numeric value greater than zero is truth. We should always check arrays as following:


if(nonEmptyArray.length){
    console.log("Truthy");
} else {
    console.log("Falsy");
}


## Ternary operator in Javascript


Ternary operator in javascript is a shorthand to write if statement. Following syntax is used:


`expression ? do_this_if_truth : do_this_if_false`


Ternary operator mostly used in conditional assignments to variables. Ternary operators may be used with exact matches or with only truthy and falsy values.


const nonEmptyArray = ['an element'];
const arrayState = nonEmptyArray.length ? "Filled": "Empty"; // arrayState is assigned "Filled"


## Assigning values to variables using OR (||) operator in Javascript


The OR (||) operator is specifically used when assigning values to variables. The left hand side value is evaluated for truthiness and it also gets assigned if true. Otherwise, right hand value is assigned. The most common use case for assignment using OR (||) operator is when we are making sure that a default value is always assigned.


const textInpuFromUser = "";
const textInput = textInpuFromUser || "No input added";


## How null coalescing operator (??) is different from OR (||) operator


The null coalescing operator (??) unlike OR (||) operator only checks for undefined or null on its left hand side value. In many cases, falsy values are legitimate to get assigned to the variable but the only check we want to place is for either undefined or null.


const optionalTextField = "";
const isFormValid = optionalTextField || "Please type in text field";


In above example, an empty string for an optional text field is perfectly valid but as we are using OR (||) operator, it evaluates empty string as false and hence assigns right side value which is an error message.


Null coalescing operator only chooses its right side value when the left hand side value is either null or undefined.


const optionalTextField = "";
const isFormValid = optionalTextField ?? "Please type in text field"; // assigns ""

const optionalTextField = undefined;
const isFormValid = optionalTextField ?? "Please type in text field"; // assigns "Please type in text field"

const optionalTextField = null;
const isFormValid = optionalTextField ?? "Please type in text field"; // assigns "Please type in text field"

```
