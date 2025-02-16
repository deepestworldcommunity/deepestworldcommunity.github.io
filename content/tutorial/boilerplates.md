---
title: "Boilerplates"
draft: false
weight: 3
---

# Boilerplates

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

## Setups 

### Default Code

```js
gameLoop();
function gameLoop () {
  attack();
  setTimeout(gameLoop, 250);
}
function attack () {
  const target = dw.findClosestMonster();
  if (!target) {
    return;
  }
  // index of Attack Rune on your skill bar
  const skillIndex = 0;
  // move your character to the monster
  dw.move(target.x, target.y);
  // show the target frame on the GUI
  dw.setTarget(target.id);
  // check for mana, range and GCD (global cooldown)
  if (!dw.canUseSkill(skillIndex, target.id)) {
    return;
  }
  dw.useSkill(skillIndex, target.id);
}
```

### Priority Coding by Hunsrak

Let me tell you an imagined story going for an Adventure in Deepest World.

My main goal is to grind for resources. Whenever my inventory gets full I want to go back to base and put all items into bank. Sometimes there are Monsters trying to kill me, so I need to defend myself. Also, there are rare or too strong monsters near my resources which I really like to grind. But I know I will be dead if try to fight them, so I will just leave the most wanted resource and retreat from these strong monsters. Exploring the map of deepest world is awesome by just moving randomly around and eventually getting far away from start point. However when walking around, I want to make sure that I don't fall in a hole or run into a wall.

Think about, how and why you would prioritize tasks in script-functions correctly depending on the story I wrote. (I didn't write the story chronological on purpose.)
I'll show you, how script-functions could be sorted by priority.

```js
const scriptFunctions = [
  retreatFromStrongMobs, //highest priority
  selfDefense,
  InventoryToBank,
  backToBase,
  checkTerrainBeforeMoving,
  moveToAndGatherResource,  //My main goal!
  findResource,
  goToMovementPoint,
  createRandomMovementPoint, //lowest priority
];
```

**Explanation:**
- `scriptFunctions` is the main part, which you'll fill with script-functions.
- The execution order always goes from top to bottom and NOT all at the same time. (no random actions)
- The script-function you want to be executed and prioritized first should be in the first line. (`scriptFunctions[0]`)

**What to do?:**  
- Write your script functions in a way, that they always return (at least one false) boolean value (return true/false).
- After declaring your script-function, add it in `scriptFunctions` and that's it.

**Details on writing script-functions using `return true` or `return false`:**
- use `return true`, if you really want to repeat executing the script function again.(Because it is important) And at the same time, ignore all other script functions after that.
- use `return false`, if there is no reason to do anything. But telling `scriptFunctions`, that the next script-function can be executed.

I'll give you an example of how a script-function could look like for attacking Monsters:

```js
async function attack() {
  const target = dw.findClosestMonster();
  if(target && dw.canUseSkill(0, target)){
    await dw.useSkill(skillIndex, target);
    console.log("Attacking Monster...");
    return true;
    //Leave this script-function and restart loop cycle to keep attacking while ignoring all other script-functions after this. (Because it is important)
  };
  if(!target){
    console.log("No target found.");
    return false;
    //Leave this and skip to the next script function because there is nothing to do.
  }
}
```

The code setup for prioritizing tasks:

```js
const scriptFunctions = [
  attack,  // Highest priority
  harvest, 
  movement,  // Lowest priority
  //add more here
];

// ----- game loop -----
let delay = 500;
setInterval(gameLoop, delay );

function gameLoop() {
  scriptFunctions.some( (task) => task() );
};

// ----- declaring script-functions -----
function attack() {};
function harvest() {};
function movement() {};
```

Or in case you want to use async/await (Promises), you could use this:

```js
setInterval(gameLoop, 250);

async function gameLoop() {
  return await [
    attack,
    harvest,
    movement,
  ].reduce(async (memo, fn) => await memo || await fn(), Promise.resolve(false))
}
async function attack() {}
async function harvest() {}
async function movement() {}
```
```js
//or this
const scriptFunctions = [
  attack,  // Highest priority
  harvest, 
  movement,  // Lowest priority
];

let delay = 500;
setInterval(gameLoop, delay);

async function gameLoop() {
  for(const script of scriptFunctions) {
    if(await script()) break
  }
}

async function attack() {}
async function harvest() {}
function movement() {}
```

All variations are pretty similar and have their own advantages and disadvantages.
