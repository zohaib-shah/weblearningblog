---
layout: post
title: "Understanding pass by reference and pass by value in Javascript"
date: 2024-02-04
categories: [JavaScript]
tags: [javascript]
---

# Understanding pass by reference and pass by value in Javascript
A variable with primitive data type is passed by value in javascript. All non-primitive data types such as arrays and objects are passed by reference in javascript.


Following are the examples of primitive data types in javascript:


```javascript
const name = "John Doe"; // string
const age = 22; // number
const isEnrolledInCourse = false; // boolean
const nationality = undefined; // undefined
const nullVar = null; // null


All above primitive data types variables are passed to functions by their values. Let's understand this with some code examples.


Pass by value code example in Javascript:


const name = "John Doe"; // string
const age = 22; // number
const isEnrolledInCourse = false; // boolean
const nationality = undefined; // undefined
const nullVar = null; // null


const capitalizeName = (n) => {
    n = n.toUpperCase();
    console.log(`The state of the parameter 'n' inside capitalizeName() = ${n}`);
};
console.log(`Variable "name" before processed by capitalizeName() = ${name}`);
capitalizeName(name);
console.log(`Variable "name" after processed by capitalizeName() = ${name}`);

// Outputs as below
// Variable "name" before processed by capitalizeName() = John Doe
// The state of the parameter 'n' inside capitalizeName() = JOHN DOE
// Variable "name" after processed by capitalizeName() = John Doe


In passed by value, a copy of the variable sent to the function. So whatever we do with that parameter inside the function, the original value of the variable is not affected.
In the code example above, even though we have converted the received argument into capital letters. The original 'name' variable is unchanged.


Pass by reference code example in Javascript:


In javascript, when a non-primitive data type variable is passed to a function, it is passed as reference. Any changes made to the passed parameter will impact the actual variable as well.


const student = {
    first: 'John',
    last: 'Doe',
}

const capitalizeStudent = (s) => {
    s.first = s.first.toUpperCase();
    s.last = s.last.toUpperCase();
};

capitalizeStudent(student);
console.log(student);

// Output
// { first: 'JOHN', last: 'DOE' }


In the above code, the `"student"` object passed to `capitalizeStudent()` function. We reassigned `first` and `last` property with capitalized version of them. This change has actually impacted the original student object.


Let's explore another code example for pass by reference in javascript. This time, we will pass an array to a function:


const studentNames = [
    {
        first: 'John',
        last: 'Doe',
    },
    {
        first: 'Foo',
        last: 'Bar',
    },
];

const capitalizeNames = (names) => {
    for(let i = 0; i

```