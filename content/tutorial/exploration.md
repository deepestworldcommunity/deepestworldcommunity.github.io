---
title: "Exploration"
draft: false
weight: 4
---

# Exploration

Deepest world is made up of tiles with three coordinates: `x`, `y`, and `z`.

Exploration is a big part of the game. You can explore the world by moving around.

## Random Exploration

Let's start with a simple exploration, the random exploration.

```js
setInterval(randomWalk, 1000)

function randomWalk() {
    let randomX = 2 * Math.random() - 1;
    let randomY = 2 * Math.random() - 1;
    dw.move(dw.c.x + randomX , dw.c.y + randomY)
}
```

`2 * Math.random() - 1` will give you a random number between -1 and 1.
This will move your character in a random direction every second.
You will explore the game world and eventually die or get stuck. But hey, you are exploring the game world! 🎉

## Straight Walk

We can move in a straight line until we hit a wall:

```js
// Start with moving to the right
let dx = 1
let dy = 0

function straightWalk () {
  while (dw.getTerrainAt(dw.character.x + dx, dw.character.y + dy, dw.character.z) > 0) {
    dx = 2 * Math.random() - 1
    dy = 2 * Math.random() - 1
  }

  dw.move(dw.character.x + dx, dw.character.y + dy)
}

function gameLoop () {
  straightWalk()
}

setInterval(gameLoop, 1000)
```

Straight walk will keep your character moving in a straight line until it hits a wall.
Then it will move in a random direction. This will make your character explore the game world in a more structured way.

## Cardinal direction movement

```js


function moveInDirection(dir) {
  const directions = {
    north: [ 0,-1], 
    east : [ 1, 0],
    south: [ 0, 1],
    west : [-1, 0],
  };
  let movePoint = {x: dw.c.x, y: dw.c.y};
  movePoint.x += directions[dir][0];
  movepoint.y += directions[dir][1];
  dw.move(movePoint.x, movePoint.y);
}

let direction = "north"
setInterval(() => {
  moveInDirection(direction);
}, 1000);
```

## Detecting Walls and Holes

You can access the tile you are standing on like this:

```js
dw.getTerrainAt(dw.c.x, dw.c.y, dw.c.z) // This will return the number of the tile you are standing in
dw.getTerrainAt(dw.c.x, dw.c.y, dw.c.z - 1) // This will return the terrain object of the tile below your character
```

The `z-1` is used to access the tile below you. This will return the terrain type, that you can look up in `dw.enums.Terrain`.

It's mostly used to make the game for appealing to the eye, but it is also be used for some gameplay mechanics.
Like passable terrain is always `<=0` and you can only dig on `dw.enums.Terrain.DIRT`.

Let's make a simple improvements to this script. We can check if there is a wall in front of us and if so, move in a different direction:

```js
setInterval(() => {
  if (dw.getTerrainAt(dw.character.x + 1, dw.character.y, dw.character.z) <= 0) {
    dw.move(dw.character.x + 1, dw.character.y)
  } else {
    dw.move(dw.character.x, dw.character.y + 1)
  }
}, 1000)
```

This will just check of a wall to the right and then move down (ignoring that there might be a wall there too).

## Drunken Exploration

There also is a slight variation to that called "Drunken Exploration".
It behaves like straight walk, but similar to a drunk person it fails to walk straight:

```js
// Start with moving to the right
let dx = 1
let dy = 0
// Drunkenness factor, 0 is no drunkenness = straightWalk, 1 is completely random = randomWalk
const DRUNKENNESS = 0.1

function drunkenExploration () {
  let angle = Math.atan2(dy, dx)
  angle += (Math.random() - 0.5) * DRUNKENNESS * Math.PI
  dx = Math.cos(angle)
  dy = Math.sin(angle)

  while (dw.getTerrainAt(dw.character.x + dx, dw.character.y + dy, dw.character.z) > 0) {
    dx = 2 * Math.random() - 1
    dy = 2 * Math.random() - 1
  }

  dw.move(dw.character.x + dx, dw.character.y + dy)
}

function gameLoop () {
  drunkenExploration()
}

setInterval(gameLoop, 250)
```

It's only a slight adjustment, but it might be a bit mathy, so let's break it down:

- `Math.atan2(dy, dx)` will give you the angle of the direction you are moving in.
- `(Math.random() - 0.5) * DRUNKENNESS * Math.PI` will give you a random angle between -DRUNKENNESS and DRUNKENNESS.
- `Math.cos(angle)` and `Math.sin(angle)` will give you the new direction you are moving in.

This will make your character explore the game world in a more structured way, but still with a bit of randomness.
