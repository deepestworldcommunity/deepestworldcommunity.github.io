---
title: "First Steps"
draft: false
weight: 2
---

# First Steps

Your character can be accessed in multiple ways:

```js
dw.character // This is the character object
dw.char // This is a shorthand for the character object
dw.c // This is a shorthand for the character object
```

Besides you, there are other entities in the game. You can access them like this:

```js
dw.entities // This is the entities object
dw.e // This is a shorthand for the entities object
```

## Movement

To move around in the world you can use the `dw.move(x, y)` function.  
You can also stop your movement with the `dw.stop()` function.

## Attacking Monsters

### Finding

To attack a monster, you first need to find it in `dw.entities` (an array of `Entity` objects).  
Deepest World provides a helper function to find the monster closest to your character: `dw.findClosestMonster()`.

```js
const target = dw.findClosestMonster();
```

You can also pass a filter function to `dw.findClosestMonster()` to check for specific criteria, 
like looking for monsters with the bad property or monsters that are attacking you.

```js
const badMonsterFilter = (e) => e.bad;
const badMonster = dw.findClosestMonster(badMonsterFilter);

const targettingMeFilter = (e) => e.targetId === dw.character.id;
const targettingMe = dw.findClosestMonster(targettingMeFilter);
```

### Use Skills

Just finding a monster isn't enough. You also need to use skills in order to attack them, 
this will be done with the function `dw.useSkill(skillIndex, target)`.

```js
const target = dw.findClosestMonster();
const skillIndex = 0;
if (target) {
  dw.useSkill(skillIndex, target);
}
```

Besides having a target there are more conditions that have to be met in order to use a skill.

* your character needs to be in range to use the skill: `dw.isInRange(skillIndex, target)`
* your character will need to have enough mana to use the skill: `dw.canPayCost(skillIndex)`
* the skill has to be off cooldown: `dw.isReady(skillIndex)`
* your character will have to target the monster (often called line of sight) - unfortunately, there is no helper function for this yet
 
The checks that have a helper function are combined in an additional helper function: `dw.canUseSkill(skillIndex, target)`

```js
const target = dw.findClosestMonster();
const skillIndex = 0; // hardcoded index of the skill you want to use
if (target && dw.canUseSkill(skillIndex, target)) {
  dw.useSkill(skillIndex, target); 
}
```

* If the skill is on cooldown or you currently have not enough mana, you can just wait.
* If you are out of range, you can move closer to the monster.
* If you are not in line of sight, you will have to move to a position where you can see the monster.

## Where To Go From Here 

* See [skills](/game-mechanics/skills/) to know more how they can be found and learned.
* To read more about movement see the [Exploration](/tutorial/exploration/) tutorial.
