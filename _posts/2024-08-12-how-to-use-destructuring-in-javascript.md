---
layout: post
title: "How to use destructuring in Javascript"
date: 2024-08-12
categories: [JavaScript]
tags: [javascript]
---

Destructuring in javascript is a syntax enhancement. It enables us to extract properties from object and create a dedicated variable for them. In this post, we will learn how to use destructuring in javascript to make the code more readable and concise.


How to destructure an object in Javascript


Lets create a simple javascript object `person` as follows:


```javascript
const person = {
	name: "John",
        email: "john@email.com"
};
```

Without destructuring, we would access a person's name using `person.name`. Accessing the `name` property through the `person` object every time can be cumbersome and doesn't contribute much to code readability.

A better approach is to extract the `name` variable beforehand and use it whenever needed.

```javascript
const { name  } = person;
console.log(name);
// logs "John"
```

After destructuring, name is an independent variable now. To destructure multiple properties from a javascript object at once, we can use following syntax.

```javascript
const person = {
	name: "John",
  email: "john@email.com"
};

const { name, email  } = person;
console.log(name);
// logs "John"
console.log(email);
// logs "john@email.com"
```

## Destructuring nested objects in Javascript


Let's convert the `person` object to contain a nested property called `course`. Now, we would like to destructure the nested object and extract the `title` property from the nested `course` field.

```javascript
const person = {
	name: "John",
  email: "john@email.com",
  course: {
  	title: "An introduction to JavaScript",
    isEnrolled: 1
  }
};

const { name, email, course: { title }  } = person;
console.log(name);
// logs "John"
console.log(email);
// logs "john@email.com"
console.log(title);
// logs "An introduction to JavaScript"
```

The syntax `{ course: { title } }` tells javascript to first access the `course` property within the `person` object, and then extract the `title` property from that nested object.

## Array destructuring in Javascript


To make javascript array handling more readable and concise, array destructuring is a handy technique. Let's see it with an example.

```javascript
const fifaRank = ['Argentina', 'France', 'Spain'];

const [firstTeam, secondTeam, thirdTeam] = fifaRank;
console.log(firstTeam);
// logs "Argentina"
console.log(secondTeam);
// logs "France"
console.log(thirdTeam);
// logs "Spain"
```

## Using rest operator in array destructuring in Javascript

The rest operator makes sure that we take care of other array values while destructuring an array.

```javascript
const fifaRank = ["Argentina", "France", "Spain"];
const [firstTeam, ...restOfTheTeams] = fifaRank;
console.log(firstTeam);
// logs "Argentina"
console.log(restOfTheTeams);
// logs ["France", "Spain"]
```

## Aliasing destructured variables in Javascript

In some cases, we may already have a variable name used in the code that conflicts with a destructured property. In such cases, we can assign a different name during destructuring. For example, if the variable `email` is already used in the code, we should give the destructured `email` property a different name.

```javascript
const email = "abc@gmail.com";
const person = {
	name: "John",
  email: "john@email.com",
};

const { email: studentEmail } = person;
console.log(studentEmail);
// logs "john@email.com"
console.log(email);
// logs "abc@gmail.com"
```

If we don't alias the destructured variable name and try to give the same name as already being used. We would get following error:

```
Uncaught SyntaxError: Identifier 'email' has already been declared
```

To solve this, we should always assign a different name to destructured variable.

## Assigning defaults while destructuring in Javascript


In some cases, we might intend to destructure a property from an object that may not even exist. We can assign a default value, as shown in the following example.

```javascript
const student = {
	name: "John",
};

const { name, thumbnail = "./default.png" } = student;
console.log(thumbnail);
// logs "./default.png"
```

And it works for array destructuring as well.

```javascript
const fifaRank = ['Argentina'];

const [firstTeam, secondTeam = "Not Available"] = fifaRank;

console.log(secondTeam);
// logs "Not Available"
```

## Destructuring nullable objects in Javascript

An object in JavaScript can be `null` or `undefined`. Let's try destructuring an object that is set to `undefined`.

```javascript
const student = undefined;
const {
  name,
  thumbnail
} = student;
console.log(thumbnail);
// Uncaught TypeError: Cannot destructure property 'name' of 'student' as it is undefined.
```

As `student` is undefined, destructuring it will result in a Uncaught TypeError:

```
Uncaught TypeError: Cannot destructure property 'name' of 'student' as it is undefined.
```

To solve this error, always handle null and undefined objects.

Lets handle the `Uncaught TypeError` using [null coalescing operator](https://www.weblearningblog.com/javascript/understanding-null-coalescing-and-ternary-operator-with-truthy-and-falsy-values-in-javascript/).

```javascript
const student = undefined;
const {
  name,
  thumbnail
} = student ?? {};
console.log(thumbnail);
```

Now, we will destructure an empty object if `student` is `undefined` or `null`. By doing this, we will not encounter any errors.

## Object destructuring in function arguments in Javascript

Another way to make your code readable is to use destructuring in function arguments. Lets explore an example with and without destructuring in function arguments.

```javascript
const displayName = (name) => {
	console.log(name);
}

const student = {
	name: "John",
  email: "john@email.com"
};
const { name } = student;
displayName(name);
```

Instead of destructuring and then providing the name to the function to display, it would be much better if the `displayName` function could handle it in its arguments. The following is a more readable version of the code above.

```javascript
const displayName = ({name}) => {
	console.log(name);
}

const student = {
	name: "John",
  email: "john@email.com"
};
displayName(student);

```