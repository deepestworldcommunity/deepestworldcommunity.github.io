---
title: "Boilerplates"
draft: false
weight: 3
---

# Boilerplates

## Default Code

The default code is the one that's present when you start the game for the first time.
You can also restore it in-game via `Esc` -> `Help` -> `Restore Default Code`.

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

Let me tell you an imagined story going for an adventure in Deepest World.

My main goal is to grind for resources. Whenever my inventory gets full I want to go back to base and put all items into bank.
Sometimes there are Monsters trying to kill me, so I need to defend myself. 
Fighting against too strong monsters can end deadly, so it is better to ignore the source, which is guarded by that strong monster and retreat from it.
Exploring the map of deepest world is awesome by just moving randomly around and eventually getting far away from starter area.
However, when walking around I want to make sure, to not fall in a hole or run into a wall.

Think about, how and why you would prioritize tasks in script-functions correctly depending on the story I wrote, which isn't chronological on purpose.
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

**Explanation**
- `scriptFunctions` is the main part, which you'll fill with script-functions.
- The execution order always goes from top to bottom and NOT all at the same time. (no random actions)
- The script-function you want to be executed and prioritized first should be in the first line.

**What to do?**  
- Write your script functions in a way, that they always return (at least one false) boolean value.
- After declaring your script-function, add it in `scriptFunctions` and that's it.

**Details on writing script-functions using `return false` or `return true`**
- use `return false`, if there is no reason to do anything. But telling `scriptFunctions`, that the next script-function can be executed.
- use `return true`, if you want to repeat executing the script function again (because it is important). And at the same time, ignore all other script functions after that.

I'll give you an example of how a script-function could look like for attacking Monsters:

```js
async function attack() {
  const target = dw.findClosestMonster();
  if (target && dw.canUseSkill(0, target)) {
    await dw.useSkill(0, target);
    console.log("Attacking Monster...");
    // restart loop cycle and ignore all other script-functions after this. (Because it is important)
    return true;
  }
  if (!target) {
    console.log("No target found.");
    // skip to the next script function because there is nothing to do.
    return false;
  }
}
```


**The code setup for prioritizing tasks**

Simplest variation

```js
const scriptFunctions = [
  // highest priority
  attack,
  harvest,
  movement,
  // lowest priority
];

setInterval(() => {scriptFunctions.some((task) => task());}, 500);

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

or more complex if you want to change delay for each loop cycle

```js
const scriptFunctions = [
  // highest priority
  attack,
  harvest,
  movement,
  // lowest priority
];

let delay = 1000;
gameLoop();
async function gameLoop() {
  while (true) {
    delay = 500; //reset delay at beginning of every loop cycle
    for (const script of scriptFunctions) {
        if (await script()) break;
    }
    await new Promise(resolve => setTimeout(resolve, delay)); //timer function
  };
}

async function attack() {
    //After sucessful attack, you can set delay equal to global cool down (gcd)
    delay = dw.constants.GCD
}
async function harvest() {}
function movement() {}
```

All variations are similar and have their own advantages and disadvantages.
