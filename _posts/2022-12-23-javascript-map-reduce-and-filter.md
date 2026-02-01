---
layout: post
title: "Javascript map, reduce and filter"
date: 2022-12-23
categories: [NodeJS]
tags: [javascript]
---

Javascript map, reduce and filter functions are some of the useful array functions in javascript. These are the higher order functions which takes a callback function as their parameter.


## Javascript map() function


The javascript map() function is an array function which iterates through the array and executes the provided callback on each element. The map function returns an array which consists of the modified elements.


map function processes each element of the array through the provided callback function. The provided callback function could be any valid javascript function which must take one argument.


### Javascript map() function on simple array:


```javascript
let numberArray = [2,5,6,7,8];
let newArr = numberArray.map(item => item * 3);
console.log(newArr);
// [ 6, 15, 18, 21, 24 ]
```

### Javascript map() function on array of objects (JSON):


The javascript map function also operates on array of objects as below:

```javascript
let students = [
    {
        name: 'john doe',
        age: 20
    },
    {
        name: 'zohaib shah',
        age: 20
    },
];

let upperCasedStudents = students.map(function(student){
    student.name = student.name.toUpperCase();
    return student;
});
console.log(upperCasedStudents);

// outputs [ { name: 'JOHN DOE', age: 20 }, { name: 'ZOHAIB SHAH', age: 20 } ]
```

We can also detach function definition as follows:

```javascript
const nameToUpperCase = (student) => {
    student.name = student.name.toUpperCase();
    return student;
};
let students = [
    {
        name: 'john doe',
        age: 20
    },
    {
        name: 'zohaib shah',
        age: 20
    },
];

let upperCasedStudents = students.map(nameToUpperCase);
console.log(upperCasedStudents);

// outputs [ { name: 'JOHN DOE', age: 20 }, { name: 'ZOHAIB SHAH', age: 20 } ]
```

Little but very important change in behavior of map should be noted here. For array of objects, map function will mutate the objects in the array. In simple array, the original array remains intact even after processed by javascript map function. Therefore, `console.log(students)` in above code snippet will also produce the similar result as `console.log(upperCasedStudents)`

## Javascript filter() function

Javascript filter function takes a callback function and pass each element of the array one by one through it. The callback function must accept one parameter and the body of the callback function must have a conditional clause (as we provide in if or while).

Based on the conditional expression, Javascript filter function decides whether to include this element in returned array or not.

```javascript
let numberArray = [2,5,6,7,8];
console.log(numberArray.filter(x => x % 2 == 0));
// outputs [ 2, 6, 8]
```

Just like map, filter function also works on array of objects.

```javascript
const isJSStudent = (student) => {
    return student.programmingLang == 'JS';
};
let students = [
    {
        name: 'john doe',
        programmingLang: 'JS',
        age: 20
    },
    {
        name: 'King John',
        programmingLang: 'JS',
        age: 20
    },
    {
        name: 'zohaib shah',
        programmingLang: 'C#',
        age: 20
    },
];

jsStudents = students.filter(isJSStudent);
console.log(jsStudents);

// Outputs
// [
//     { name: 'john doe', programmingLang: 'JS', age: 20 },
//     { name: 'King John', programmingLang: 'JS', age: 20 }
// ]
```

## Javascript reduce() function

Javascript reduce function processes an array and produce a single result out of it. The reduce function takes a callback function. This time, the callback function takes two parameters. The first parameter is the result from the callback function execution during previous iteration. The second parameter is the current element.

```javascript
let numberArray = [1,2,3];
let sum = numberArray.reduce((lastResult, currentValue) => {
    return lastResult + currentValue;
});
console.log(sum);
// 6

```