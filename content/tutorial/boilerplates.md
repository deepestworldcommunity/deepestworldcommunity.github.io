---
title: "Boilerplates"
draft: false
weight: 3
---

# Boilerplates

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

```js
let delay = 500
setInterval(botLoop, delay)

function botLoop () {
  console.log('delay:', delay)
  scriptFunctions()
}

function scriptFunctions () {
  if (attack()) {return} // Highest priority
  if (harvest()) {return}
  if (movement()) {return} // Lowest priority
  //add more script-functions here.
}

// ----- script-functions -----
function attack () {}

function harvest () {}

function movement () {}
```

This boilerplate assumes that you have multiple sub-tasks that you can prioritize. 
If a task is successful, it will return `true` and the next task will not be executed. 
This way you can prioritize your tasks. 

A shorter example would probably look like this:

```js
setInterval(gameLoop, 250);

function gameLoop() {
  [
    attack, 
    harvest, 
    movement,
  ].some((task) => task());
}

function attack () {}

function harvest () {}

function movement () {}
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

All variations are pretty similar and have their own advantages and disadvantages.
