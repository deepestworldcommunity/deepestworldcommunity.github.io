---
title: "Introduction Part 2"
draft: false
weight: 1
---
# Introduction Part 2

## Try - Catch

In JavaScript, the `try {code} catch(error) {handle}` statement is used to handle runtime errors, also known as exceptions. This statement allows you to execute a block of code that might throw an error and then handle the error gracefully without stopping the execution of the program.

```js
try {
  // Code that might throw an error
  noSuchVariable; // This will throw a ReferenceError
} catch (err) {
  // Code to handle the error
  console.error("An error occurred:", err);
}
```

The catch block receives an error object as an argument, which contains information about the error. This object typically has two main properties:  
`name`: The name of the error, such as ReferenceError, TypeError, etc.  
`message`: A string that describes the error.

## async () => await Promise()

In JavaScript, `async` and `await` are used to handle asynchronous operations in a more readable and synchronous-like manner. These features are built on top of promises, which are objects representing the eventual completion or failure of an asynchronous operation.

A promise in JavaScript can be in one of three states: pending, fulfilled, or rejected. When an asynchronous operation completes, the promise is either fulfilled with a value or rejected with an error. Promises are useful for handling asynchronous operations and chaining multiple asynchronous operations together.

```js
async function asyncAttack() {
  const value = await dw.useSkill(skillIndex, target); //Returns <Promise> with <undefined> as resolve value
  if (value === undefined) {
    return 'Attacking successful!';
  }
  throw Error('Attacking failed for some reason.');
}
```

## Calling Functions with or without parentheses?

Calling with Parentheses:  
When you call a function with parentheses (e.g., myFunction()), the function is executed immediately. The code inside the function runs, and if the function returns a value, that value is returned to the caller.

Calling without Parentheses:  
When you call a function without parentheses (e.g., myFunction), you are not executing the function. Instead, you are referring to the function itself. This is often used when you want to pass the function as a reference to another function or method, such as in event handling or as a callback.

```js
setInterval(gameLoop, 1000) //setTimeout() is the call while gameLoop is referred.
```

*Better example?*

## Timers

JavaScript timers are used to execute code after a specified delay or at regular intervals. The primary functions for timers in JavaScript are `setInterval` and  `setTimeout`.

### setInterval

`setInterval` Executes a function or code block repeatedly at specified intervals. To stop the execution of `setInterval`, you can use `clearInterval(intervalID)`, where `intervalID` is the identifier returned by `setInterval`.

```js
let intervalId = setInterval(gameLoop, 500);

async function gameLoop() {
  try {
    await dw.useSkill(0, dw.findClosestMonster())
  } catch(err) {
    console.error(`Oops, something went wrong. Stopping gameLoop...\ ${err.message}`)
    clearInterval(intervalId);
  }
}
```

### setTimeout

`setTimeout` Executes a function or code block ONCE after a specified delay. You might have to put it in a recursive function (for looping) to run your code properly.

```js
gameLoop()

function gameLoop() {
  attack();
  setTimeout(gameLoop, 500);
}
```

### sleep

Using await with setTimeout directly is not possible because `setTimeout` returns a number, not a promise. To use `setTimeout` in an async function with await, you need to wrap setTimeout in a promise.

```js
const sleep = async (ms=500) => new Promise(resolve => setTimeout(resolve,ms))

gameLoop();

async function gameLoop() {
  while(true) {
    await attack()
    await sleep()
  }
}
```

### Adjusting the Timing

You might have noticed that doing so every second is a bit slow. You can speed it up by changing the interval to 250ms.

You can choose any value you like, but keep in mind that the game will only update the entities every 50ms.
So choosing smaller values will eventually result in invalid moves, or even a disconnect.

```js
let delay = 1000;
delay = 250
setInterval(gameLoop, delay)
```
