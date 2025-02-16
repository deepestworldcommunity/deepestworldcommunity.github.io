---
title: "Boilerplates"
draft: false
weight: 3
---

# Boilerplates

## Default Code

The default code is the one that's present when you first start the game, 
you can also restore in in-game via `Esc` -> `Help` -> `Restore Default Code`. 

```js
gameLoop();

function gameLoop() {
  attack();

  setTimeout(gameLoop, 250);
}

function attack() {
  const target = dw.findClosestMonster();
  if (!target) {
    return;
  }

  // index of attack skill on your skill bar
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

## Priority Coding by Hunsrak

Let me tell you an imagined story going for an Adventure in Deepest World.

My main goal is to grind for resources. Whenever my inventory gets full I want to go back to base and put all items into bank. 
Sometimes there are Monsters trying to kill me, so I need to defend myself. 
Also, there are rare or too strong monsters near my resources which I really like to grind. 
But I know I will be dead if try to fight them, so I will just leave the most wanted resource and retreat from these strong monsters. 
Exploring the map of deepest world is awesome by just moving randomly around and eventually getting far away from start point. 
However, when walking around I want to make sure that I don't fall in a hole or run into a wall.

Think about, how and why you would prioritize tasks in script-functions correctly depending on the story I wrote. (I didn't write the story chronological on purpose.)
I'll show you, how script-functions could be sorted by priority.

```js
const scriptFunctions = [
  // highest priority
  retreatFromStrongMobs, 
  selfDefense,
  InventoryToBank,
  backToBase,
  checkTerrainBeforeMoving,
  moveToAndGatherResource,  // my main goal!
  findResource,
  goToMovementPoint,
  createRandomMovementPoint,
  // lowest priority
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
  if (target && dw.canUseSkill(0, target)) {
    await dw.useSkill(skillIndex, target);
    console.log("Attacking Monster...");
    // Leave this script-function and restart loop cycle to keep attacking while ignoring all other script-functions after this. (Because it is important)
    return true;
  }
  if (!target) {
    console.log("No target found.");
    // Leave this and skip to the next script function because there is nothing to do.
    return false;
  }
}
```

The code setup for prioritizing tasks:

```js
// add more here
const scriptFunctions = [
  // highest priority
  attack,  
  harvest, 
  movement,
  // lowest priority
];

// ----- game loop -----
let delay = 500;
setInterval(gameLoop, delay );

function gameLoop() {
  scriptFunctions.some((task) => task());
};

// ----- declaring script-functions -----
function attack() {}
function harvest() {}
function movement() {}
```

Or in case you want to use async/await (Promises), you could use this:

```js
setInterval(gameLoop, 250);

async function gameLoop() {
  return await [
    attack,
    harvest,
    movement,
  ].reduce(async (memo, fn) => await memo || await fn(), Promise.resolve(false));
}
async function attack() {}
async function harvest() {}
async function movement() {}
```

or this

```js
const scriptFunctions = [
  // highest priority
  attack,  
  harvest, 
  movement,
  // lowest priority
];

let delay = 500;
setInterval(gameLoop, delay);

async function gameLoop() {
  for (const script of scriptFunctions) {
    if (await script()) break;
  }
}

async function attack() {}
async function harvest() {}
function movement() {}
```

All variations are pretty similar and have their own advantages and disadvantages.
