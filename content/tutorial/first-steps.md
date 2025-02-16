---
title: "First Steps"
draft: false
weight: 2
---

# First Steps

Your character can be accessed in multiple ways:

```js
dw.character // This is the character object
dw.c // This is a shorthand for the character object
```

Besides you, there are other entities in the game. You can access them like this:

```js
dw.entities // This is the entities object
dw.e // This is a shorthand for the entities object
```

## Attacking monsters

### Finding

To attack a Monster, first you have to catch the monster in `dw.entities`, which is an array of (Entity) objects.  
Deepest World provides you with a helper function that finds the closest monster to `dw.character`.  
`dw.findClosestMonster() //returns (monster) object closest to your character`

```js
const target = dw.findClosestMonster();
// option 2: Using a filter function to check for criterias
const filterFn = (e) => Boolean(e.bad); //not all monsters are bad in Deepest World
const monster = dw.findClosestMonster(filterFn);
```

### Use skills

Just finding a monster isn't enough. You also need to use skills in order to attack them.  
`dw.useSkill(skillIndex, target) //returns <Promise> with <undefined> as resolve value`

```js
const target = dw.findClosestMonster();
const skillIndex = 0;
if(target) {
  dw.useSkill(skillIndex, target); //might throw an error
}
```

Sometimes `dw.useSkill(skillIndex, target)` will throw an error for different reasons.
Deepest World has a function, that will check if you're able to use the skill on target.  
`dw.canUseSkill(skillIndex, target) //returns <boolean>`

```js
const target = dw.findClosestMonster();
const skillIndex = 0;
if(target && dw.canUseSkill(skillIndex, target)) {
  dw.useSkill(skillIndex, target); //won't throw an error anymore after adding the check 
}
```

Using `async () => {await Promise(...)}` . 
There are casting skills that can be "awaited" until they fire its projectile.
Kamehamehas if that explains it better. (refering on fighting style from animes like dragonball)

```js
async function kamehameha() {
  const target = dw.findClosestMonster();
  const skillIndex = 0;
  if(target && dw.canUseSkill(skillIndex, target)) {
    await dw.useSkill(skillIndex, target);
  }
}
```

Your skills can be accessed with `dw.c.skills`. It returns an array of (Skill) objects.
See [skills](https://community.deepestworld.com/game-mechanics/skills/) to know more how they can be found and learned.

## Gathering resources

The same process like finding monsters in Deepest World is done with gathering resources too, but different.
Let's find the closest resource that match the criterias in `dw.isGatherable(entity)`, which returns boolean.

```js
const resource = dw.findClosestEntity(e => dw.isGatherable(e));
if(resource && dw.isInRange(null, resource) && dw.isReady()) {
  dw.gather(resource);
}
```

Or Simply look for trees with this function.

```js
dw.findClosestTree() // This will return the closest tree to your character
```

## Movement

You can also interact with the game world by moving around:

```js
dw.move(0,0); //your bot moves straight to (x,y) coordinates.
dw.stop(); //stops your character moving
```

## find, move, gather and loop

After you learned how to find entities, attack or gather them and how to move around Deepest World, it's time to go a step further. We'll work with an example for chopping trees.

```js
function chopTree() {
  const tree = dw.findClosestTree();
  if(tree) {
    dw.move(tree.x, tree.y);
    if(dw.isInRange(null, tree) && dw.isReady()) {
      //dw.stop;
      dw.gather(resource);
    }
  }
}
```

We want to execute this code multiple times, so let's add a loop.  
There are multiple ways to loop code properly which are explained in *advanced javascript*  
But right now, we'll use `setInterval()` for looping in this example.

```js
setInterval(chopTree, 250); //repeats executing chopTree() every 250 milliseconds
```

And that's it, Look how your bot is running from tree to tree and chopping them! Have fun coding!
