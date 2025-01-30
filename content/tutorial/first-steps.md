---
title: "First Steps"
draft: false
weight: 2
---

# First Steps

You will interact with the game through the API. It is exposed as `dw` for DeepestWorld.
You can access it from the console in the developer tools or from a script.

```js
// This will log "Hello World!" to the game log
dw.log("Hello World!");
```

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

There already are a couple of useful helper functions available:

```js
dw.findClosestMonster() // This will return the closest monster to your character
dw.findClosestTree() // This will return the closest tree to your character
```

The world around you is made up of tiles with three coordinates: `x`, `y`, and `z`. You can access the tile you are
standing on like this:

```js
dw.getTerrainAt(dw.character.x, dw.character.y, dw.character.z) // This will return the terrain object of the tile you are standing in
dw.getTerrainAt(dw.character.x, dw.character.y, dw.character.z - 1) // This will return the terrain object of the tile you are standing on
```

The `z-1` is used to access the tile below you. This will return the terrain type, that you can look up in
`dw.enums.Terrain`.

It's mostly used to make the game for appealing to the eye, but it is also be used for some gameplay mechanics.
Like passable terrain is always `<=0` and you can only dig on `dw.enums.Terrain.DIRT`.

## Moving Around

You can also interact with the game world by moving around:

```js
dw.move(dw.character.x + 1, dw.character.y) // This will move your character one tile to the right
dw.move(dw.character.x, dw.character.y + 1) // This will move your character one tile down
```

Let's see how far we can get with that. When playing the game, you constantly look what to do next.
To simulate this behavior, we can use a `setInterval` function to move our character around:

```js
setInterval(() => {
  dw.move(dw.character.x + 1, dw.character.y)
}, 1000)
```

This will move your character one tile to the right every second. You will either die or hit a wall pretty soon.
But hey, you are moving your character around in the game world!

## Detecting Walls

Let's make a simple improvements to this script. We can check if there is a wall in front of us and if so, move in a
different direction:

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

## Random Exploration

Looks like the game world has changed a bit and walls appear a bit later in the game.
So maybe let's start with a simple exploration, the random exploration.

```js
setInterval(() => {
  dw.move(dw.character.x + 2 * Math.random() - 1, dw.character.y + 2 * Math.random() - 1)
}, 1000)
```

`2 * Math.random() - 1` will give you a random number between -1 and 1.
This will move your character in a random direction every second.
You will explore the game world and eventually die or get stuck. But hey, you are exploring the game world! 🎉

## Adjusting the Timing

You also might have noticed that doing so every second is a bit slow. You can speed it up by changing the interval to
250ms:

```js
setInterval(() => {
  dw.move(dw.character.x + 2 * Math.random() - 1, dw.character.y + 2 * Math.random() - 1)
}, 250)
```

You can choose any value you like, but keep in mind that the game will only update the entities every 50ms.
So choosing smaller values will eventually result in invalid moves, or even a disconnect.

## Extracting Behavior

Until now, we have used the arrow function, but let's create a game loop that we can stop later on:

```js
function gameLoop () {
  dw.move(dw.character.x + 2 * Math.random() - 1, dw.character.y + 2 * Math.random() - 1)
}

setInterval(gameLoop, 250)
```

We can also go a step further and extract the movement behavior:

```js
function randomWalk () {
  dw.move(dw.character.x + 2 * Math.random() - 1, dw.character.y + 2 * Math.random() - 1)
}

function gameLoop () {
  randomWalk()
}

setInterval(gameLoop, 250)
```

This will make it easier to add more behavior later on. Let's add another exploration method.

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

function randomWalk () {
  dw.move(dw.character.x + 2 * Math.random() - 1, dw.character.y + 2 * Math.random() - 1)
}

function gameLoop () {
  straightWalk()
}

setInterval(gameLoop, 250)
```

Straight walk will keep your character moving in a straight line until it hits a wall.
Then it will move in a random direction. This will make your character explore the game world in a more structured way.

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

## Visualizing the Direction

Since we humans tend to be visual people it might be better to show the current direction inside the game.
You actually can draw on the game screen:

```js
dw.on('drawOver', (ctx, cx, cy) => {
  ctx.lineWidth = 4;
  ctx.fillStyle = 'red';
  ctx.beginPath();
  ctx.moveTo(dw.toCanvasX(cx), dw.toCanvasY(cy));
  ctx.lineTo(dw.toCanvasX(cx + dx), dw.toCanvasY(cy + dy));
  ctx.stroke();
});
```

This will draw a red line in the direction your character is moving in.
It's not perfect, but it will give you a rough idea of where your character is going.
If you want to learn more about what's possible with the Canvas2D API, you can check out
the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D).
There also is a `drawUnder` event that will draw under the game world, which we will come back to soon.
