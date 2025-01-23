---
title: "Introduction"
draft: false
---
# Introduction

So you want to learn how to make a game AI? Or maybe programming in general?
Already know JavaScript, then head over to the [First Steps](/tutorial/first-steps).

Computer code is a set of instructions that a computer can execute. It reads input, does some processing, and then outputs a result.

Programming for DeepestWorld is primarily done in JavaScript, you can program in other languages, but the game is built with JavaScript in mind.

## Debugging
In Deepest World you can debug your code via DevTools in Console tab by pressing F12 or ctrl+shift+I.
So lets start with the basics of programming in JavaScript. Here is a simple program that outputs "Hello World!" to the console.
```js
console.log("Hello World!");
```

It actually omits the first step of reading input, but that's okay for now. The `console.log` function is a built-in function in JavaScript that outputs text to the console. The text is enclosed in quotes, which is how you define a string in JavaScript.
Strings can either be written with single quotes, double quotes, or backticks. The backticks are used for string interpolation, which we will cover later.

DeepestWorld has its own function to log to the game: `dw.log`. You can use it like this:
```js
dw.log("Hello World!");
```

Alternativly, you can hover your mouse cursor over an entity and press "e" ingame. 
It will open a window named "Inspector" where you'll get everything that you could see with `console.log()` or `dw.log()` too.  

## Variables and datatypes
Lets move on to variables. A variable is a container for a value. You can think of it as a box that holds a value. You can give it a name and assign a value to it. Here is an example:

```js
let name = "World";
dw.log(`Hello ${name}!`);
```

In this example, we define a variable called `name` and assign it the value `"World"`. We then use the variable in the `dw.log` function by enclosing it in `${}`. This is called string interpolation.
This requires the use of backticks instead of quotes. (Below button "F10")

You can also change the value of a variable after it has been defined. Here is an example:

```js
let name = "World";
dw.log(`Hello ${name}!`);
name = "DeepestWorld";
dw.log(`Hello ${name}!`);
```

Besides strings, you can also store numbers or booleans in variables. Here is an example:

```js
const isTrue = true;
let number = 42;
dw.log(`The answer to life, the universe, and everything is ${number}.`);
```

The `const` keyword is used to define a constant variable. This means that the value of the variable cannot be changed after it has been defined. The `let` keyword is used to define a variable that can be changed.

## Comments
The `//` is used to write comments in JavaScript. Comments are ignored by the computer and are used to explain the code to other programmers.
There are also multi-line comments that are written between `/*` and `*/` like this.

```js
// This is a single-line comment
/*
This is a multi-line comment.
It can span multiple lines.
*/
/**
 * This is a JSDoc comment.
 * It is used to document variables, functions and classes.
 * It also can be used for type hinting.
 */
/** @type {number} */
const number = 42;
/** @type {string} */
let name = "World";
/** @type {boolean} */
const isTrue = true;
```

JSDoc comments are used to document your code. They are written between `/**` and `*/`. They can be used to document variables, functions, and classes. They can also be used for type hinting, which is useful for debugging and code completion.
JavaScript is a dynamically typed language, which means that you don't have to specify the type of a variable when you define it. However, you can use JSDoc comments to specify the type of a variable.

Lets' move on to functions. A function is a block of code that can be called by name. It can take input, do some processing, and return a result. Here is an example:

```js
/**
 * This is a function that takes a name as input and returns a greeting.
 * @param {string} name
 * @returns {string}
 */
function greeting(name) {
  return `Hello ${name}!`;
}

console.log(greeting("World"));
```
The JSDoc comment above the function is completely optional, but it is good practice to document your code. It specifies that the function takes a string as input and returns a string as output.

## Functions
Functions can also be defined as arrow functions. Here is an example:

```js
/**
 * This is an arrow function that takes a name as input and returns a greeting.
 * @param {string} name
 * @returns {string}
 */
const greeting = (name) => {
  return `Hello ${name}!`;
};
```

They can be written in a more concise way if they only have one statement. Here is an example:

```js
/**
 * This is an arrow function that takes a name as input and returns a greeting.
 * @param {string} name
 * @returns {string}
 */
const greeting = (name) => `Hello ${name}!`;
```

## Arrays
Syntax: `let array = []`
An array is a collection of elements/values. 
It works with indexes to access their elements. An Index of 0 means the first element, 1 the second element, 2 the third, and so on...
Here is an example:
```js
let numbers = [4,2,3,1,0]
console.log(numbers[0]) //returns 4

let index = 3
console.log(numbers[index]) //returns 1
```
In Deepest World `dw.entities` returns an array of (entity) objects.

We haven't covered array methods yet, but arrow functions can help finding elements in an array. 

### Array methods
The `filter` function is used to find elements in an array that match a condition. The `find` function is used to find the first element in an array that matches a condition.
We also introduced a bunch of operators in this example:
* The `%` operator is used to find the remainder of a division.
* The `==` operator is used to simply compare two values.  `console.log(1 == "1") ` returns true.
* The `===` operator is used to deeply compare two values. `console.log(1 === "1")` returns false.
* The `>` operator is used to compare two values.
* The `&&` operator is used to combine two conditions.

```js
/** @type {number[]} */
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter((number) => number % 2 === 0);
const bigNumbers = numbers.filter((number) => number > 3);
const bigEvenNumbers = numbers.filter((number) => number % 2 === 0 && number > 3);
const three = numbers.find((number) => number === 3);
```

## if's and loops
So we already covered conditions, so the next step would be control structures. A control structure is a block of code that can change the flow of a program. 
There are three main control structures in JavaScript: `if`, `for`, and `while`. Here are a few examples:

```js
let number = 5;
if (number === 42) {
  dw.log("The answer to life, the universe, and everything is 42.");
} else {
  dw.log("That's not the answer to life, the universe, and everything.");
}

for (let i = 0; i < 5; i++) {
  dw.log(`The current number is ${i}.`);
}

while (number > 0) {
  dw.log(`The current number is ${number}.`);
  number--;
}
```

We didn't cover everything yet, but this should give you a good start. 
If you want to learn more about JavaScript, you can check out the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript). 
They have a lot of resources on JavaScript and web development in general.

But we are here do program for DeepestWorld, so lets move on to the next tutorial: [First Steps](/tutorial/first-steps).