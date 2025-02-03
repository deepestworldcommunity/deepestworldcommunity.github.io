---
title: "Introduction"
draft: false
weight: 1
---
# Introduction

So you want to learn how to make a game AI? Or maybe programming in general?
Already know JavaScript, then head over to the [First Steps](/tutorial/first-steps).

Computer code is a set of instructions that a computer can execute. It reads input, does some processing, and then outputs a result.

Programming for DeepestWorld is primarily done in JavaScript, you can program in other languages, but the game is built with JavaScript in mind.

## Debugging

DeepestWorld is a browser game, so either in the browser, the steam app or a custom electron app you will have access to the developer tools. 
You can open them either by `F12` (Windows), `Command + Option + I` (Mac) or `Ctrl + Shift + I` (*NIX). 
In the Console tab you can try out some code.

So lets start with the basics of programming in JavaScript. 
Here is a simple program that outputs "Hello World!" to the console.

```js
console.log("Hello World!");
```

It actually omits the first step of reading input, but that's okay for now.
The `console.log` function is a built-in function in JavaScript that outputs text to the console. 
The text is enclosed in quotes, which is how you define a string in JavaScript.
Strings can either be written with single quotes, double quotes, or backticks. 
The backticks are used for string interpolation, which we will cover later.

While `console` is a generic JavaScript object you can use,
DeepestWorld has its own function to log to the game: `dw.log`. You can use it like this:

```js
dw.log("Hello World!");
```

To debug entities there is also the in-game "Inspector". 
This window can be opened by hovering over the entity in question and pressing `E`. 
You will see details for this entity.  

## Variables and Data Types

Let's move on to variables. A variable is like a container that holds a value. 
You can think of it as a box where you store information.
Variables allow you to store, update and reuse values in your code.

To declare a variable in JavaScript, you can use let, const, or var. The most commonly used are let and const.

```js
let name = "World";
dw.log(`Hello ${name}!`);
```

In this example, we define a variable called `name` and assign it the value `"World"`. 
We then use the variable in the `dw.log` function by enclosing it in `${}`. 
This is called string interpolation and requires the use of backticks instead of quotes.

You can also change the value of a variable after it has been defined. Here is an example:
A variable declared with let can have its value changed after it has been initialized.

```js
let name = "World";
dw.log(`Hello ${name}!`);

name = "DeepestWorld";
dw.log(`Hello ${name}!`);
```

If you don't want a variable to change after it's been assigned, use const:

```js
const isTrue = true;
let number = 42;
dw.log(`The answer to life, the universe, and everything is ${number}.`);
```

## Comments

Comments are used to explain code and make it more readable for developers. JavaScript provides two types of comments:

### Single-line Comments

A single-line comment starts with //. Everything after // on the same line is ignored by JavaScript.

```js
// This is a single-line comment
console.log("Hello, World!"); // This is also a comment
```

### Multi-line Comments

A multi-line comment is written between `/*` and `*/` and can span multiple lines.

```js
/*
This is a multi-line comment.
It can span multiple lines.
Useful for explaining complex code.
*/
```

### JSDoc Comments

JSDoc comments are special multi-line comments used for documentation, type hinting, and better code assistance in editors.

```js
/**
 * This is a JSDoc comment.
 * It documents variables, functions, and classes.
 * It also provides type hinting.
 */

/** @type {number} */
const number = 42;

/** @type {string} */
let name = "World";

/** @type {boolean} */
const isTrue = true;
```

JSDoc is useful because JavaScript is dynamically typed, meaning variables do not require explicit type declarations. 
However, specifying types in JSDoc comments improves code readability and reduces errors.

### JSDoc with Functions

```js
/**
 * Returns a greeting message.
 * @param {string} name - The name to greet.
 * @returns {string} The greeting message.
 */
function greeting(name) {
  return `Hello, ${name}!`;
}

console.log(greeting("World"));
```

While JSDoc comments are optional, using them is considered good practice for maintaining clear and structured code.

## Functions

Functions are reusable blocks of code that perform a specific task. They can take inputs, process them, and return a result.

### Declaring Functions

Functions in JavaScript can be declared in different ways:

#### Using the function Keyword

```js
function greet() {
  console.log("Hello!");
}
```

#### Using Function Expressions

```js
const greet = function(name) {
  console.log(`Hello, ${name}!`);
};
```

#### Using Arrow Functions (ES6+)

Arrow functions provide a shorter syntax:

```js
const greet = (name) => {
  console.log(`Hello, ${name}!`);
};
```

If the function has a single expression, you can omit the braces {} and return keyword:

```js
const greet = (name) => `Hello, ${name}!`;
console.log(greet("World")); // "Hello, World!"
```

### The return Statement

Functions can return values using the return keyword:

```js
function sum(a, b) {
  return a + b;
}

console.log(sum(5, 3)); // 8
```

A function stops executing immediately when it encounters a return statement.

```js
function checkNumber(number) {
  if (number === 0) {
    return `${number} is exactly zero.`;
  }
  
  if (number > 0) {
    return `${number} is a positive number.`;
  }
  
  return `${number} is a negative number.`;
}

console.log(checkNumber(-4)); // "-4 is a negative number."
```

### Calling Functions

You call a function by using its name and passing arguments (if required):

```js
let firstName = "Deepest";
let lastName = "World";
console.log(greet(firstName, lastName)); // "Hello, Deepest World!"
```

### Nested Functions

A nested function is a function defined inside another function. Inner functions can access variables from the outer function.

```js
function conversation(name) {
  function greet() {
    console.log(`Hello, ${name}!`);
  }

  function goodbye() {
    console.log(`Goodbye, ${name}!`);
  }

  greet();
  goodbye();
}

conversation("World");
// Output:
// "Hello, World!"
// "Goodbye, World!"
```

## Arrays

An array is a collection of values. It can contain strings, numbers, objects, or even other arrays.

```js
const numbers = [4, 2, 3, 1, 0];
console.log(numbers[0]); // 4

const index = 3;
console.log(numbers[index]); // 1
```

### Array Methods

JavaScript provides many useful array methods:

### filter()

Filters elements based on a condition.

```js
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4]
```

#### find()

Finds the first element that matches a condition.

```js
const three = numbers.find(num => num === 3);
console.log(three); // 3
```

## Objects

Objects store data in key-value pairs. Keys are always strings, and values can be any data type.

```js
const person = {
  firstName: "John",
  lastName: "Doe",
  age: 25,
  getFullName: function() {
    return this.firstName + " " + this.lastName;
  },
};

console.log(person.getFullName()); // "John Doe"

let key = "age";
console.log(person[key]); // 25
```

## Control Structures

Control structures change the flow of a program.

### if Statement

Executes code only if a condition is true.

```js
let number = 5;
if (number === 42) {
  console.log("The answer to life, the universe, and everything is 42.");
} else {
  console.log("That's not the answer.");
}
```

### for Loop

Repeats a block of code a specific number of times.

```js
for (let i = 0; i < 5; i++) {
  console.log(`Iteration: ${i}`);
}
```

### while Loop

Repeats a block of code while a condition is true.

```js
let count = 3;
while (count > 0) {
  console.log(`Countdown: ${count}`);
  count--;
}
```

## Operators

We introduced several operators:

| Operator | Description                    | Example         | Output |
|----------|--------------------------------|-----------------|--------|
| %	       | Remainder	                     | 5 % 2	          | 1      |
| ==	      | Loose equality (ignores type)  | 	1 == "1"	      | true   |
| ===	     | Strict equality (checks type)	 | 1 === "1"	      | false  |
| >        | Greater than                   | 5 > 3           | true   |
| &&       | Logical AND                    | true && false   | false  |
| \|\|     | Logical OR                     | true \|\| false | true   |

## Further Learning

This guide introduced comments, functions, arrays, objects, and control structures. If you want to dive deeper, check out:
•	MDN JavaScript Documentation
•	JavaScript Info

But since we’re programming for DeepestWorld, let’s move on to the next tutorial: [First Steps](/tutorial/first-steps)
