---
title: "Boilerplates"
draft: false
weight: 3
---

# Boilerplates

## Try{} catch(error){}
Explanation here...

## Timers
setTimeout(function,ms)...  
setInterval(function,ms) and clearInterval(intervalId)...  

## Await async () => Promise()
Explanation here...

## Calling Functions with or without parentheses
Calling with Parentheses:
When you call a function with parentheses (e.g., myFunction()), the function is executed immediately. 
The code inside the function runs, and if the function returns a value, that value is returned to the caller.

Example...?

Calling without Parentheses: 
When you call a function without parentheses (e.g., myFunction), you are not executing the function.
Instead, you are referring to the function itself.
This is often used when you want to pass the function as a reference to another function or method, such as in event handling or as a callback.

Example...?

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

Let me tell you an imagined story when going for an Adventure in Deepest World.

My main goal is to grind for resources.
Whenever my inventory gets full I want to go back to base and put all items into bank.  
Sometimes there are Monsters trying to kill me, so I need to defend myself.  
Also, there are some rare or too strong monsters near my resources which I really like to grind.  
But I know I will be dead if try to fight them, so I will just leave the most wanted resource and retreat from strong monsters.  
Exploring the map of deepest world is awesome by just moving randomly around and eventually getting far away from start point.  
However when walking around, I want to make sure that I don't fall in a hole or run into a wall.

Now I'll show, how script-functions could be sorted in a specific order depending on the story I wrote.  
Think about it, how and why you would prioritize tasks in script-functions correctly. (I didn't write the story chronological on purpose.)
```js
const scriptFunctions = [
  retreatFromStrongMobs, //highest priority
  selfDefense,
  emptyInventory,
  backToBase,
  harvestResource  //My main goal!
  findResource,
  checkTerrainBeforeMoving,
  goToMovementPoint,
  randomMovementPoint, //lowest priority
]
```
#### Explanation:
- `scriptFunctions` is the main part, which you'll fill with script functions.
- The execution order always goes from top to bottom and NOT all at the same time. (no random actions)
- The script function you want to be executed and prioritized first should be in the first line. (`scriptFunctions[0]`)  
#### What to do?:  
- Write your script functions in a way, that they always return (at least one false) boolean value at the end (return true/false).
- After declaring your script-function add it in `scriptFunctions`
- And that's it.
#### Details about using `return true` or `return false` inside script-function:
- use `return true`, if you really want to repeat executing the script function again AND ignore all other script functions. (Because it is important)
- use `return false`, if there is no reason to do anything. But telling `scriptFunctions`, that the next script-function can be executed.  

I'll give you an example of how a script function can look like:
```js
//script function for attacking monsters
function attack() {
    const target = dw.findClosestMonster();
    const skillIndex = 0;
    if(target && dw.canUseSkill(skillIndex, target)){
        dw.useSkill(skillIndex, target);
        console.log("Attacking Monster...");
        return true;
        //Leave this function and restart loop cycle to keep attacking and ignore all other script functions
    };
    if(!target){
        console.log("No target found.");
        return false;
        //Leave this function and skip to next script function because there is nothing to do.
    };
};
```

The code setup for prioritizing tasks:
```js
const scriptFunctions = [
    attack,  // Highest priority
    harvest, 
    movement,  // Lowest priority
]

// ----- game loop -----
let delay = 500;
setInterval(gameLoop, delay );

function gameLoop() {
  scriptFunctions.some( (task) => task() );
};

// ----- script-functions -----
function attack() {};

function harvest() {};

function movement() {};
```
END  
//remove1 start  
This boilerplate assumes that you have multiple sub-tasks that you can prioritize. 
If a task is successful, it will return `true` and the next task will not be executed. 
This way you can prioritize your tasks. 

A shorter example would probably look like this:

```js
let delay = 500;
setInterval(gameLoop, delay);

function gameLoop() {
  [
    attack,
    harvest,
    movement,
  ].some( (task) => task() );
};

// ----- script-functions -----
function attack () {}

function harvest () {}

function movement () {}
```
//remove1 end

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

All variations are pretty similar and have their own advantages and disadvantages.
