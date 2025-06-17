---
title: "Drawing"
draft: false
weight: 5
---

# Drawing in Deepest World

In deepest world there is a feature, that allows you to draw your game screen with HTML-canvas-elements.  
It is an eventlistener, that keeps drawing the game what ever you write inside.

```js
dw.on('drawUnder', (ctx, cx, cy) => {
    //drawing under your character
});

dw.on('drawOver', (ctx, cx, cy) => {
    //drawing over your character
});
```

`ctx` is short for `document.getElementById('canvasName').getContext('2d');`, which allows you to draw with HTML-canvas-elements.  
`cx` is the center position on x-axis and `cy` on y-axis. The values are given as unit size in deepest world.
Before using drawing elements you need to convert them to pixel position like this:

```js
const pixelX = dw.toCanvasX(unitX)
const pixelY = dw.toCanvasY(unitY)
//You can choose any (x,y) position in deepest world where you like to draw on.
```

## Draw line to next target

```js
dw.on('drawUnder', (ctx, cx, cy) => {
    const target = dw.findClosestMonster();
    if(target) {
        drawLine(cx,cy ,target.x,target.y)
        function drawLine(x1,y1,x2,y2) {
            // 1. convert to pixel position
            x1 = dw.toCanvasX(x1);
            y1 = dw.toCanvasY(y1);
            x2 = dw.toCanvasX(x2);
            y2 = dw.toCanvasY(y2);
            // 2. Then draw
            ctx.strokeStyle = "red";
            ctx.lineWidth = 5;
            ctx.beginPath();
            ctx.moveTo(x1,y1);
            ctx.lineTo(x2,y2);
            ctx.closePath();
            ctx.stroke();
        }
    }
});
```

## Visualizing your direction

Since we humans tend to be visual people it might be better to show the current direction inside the game.

```js
dw.on("drawOver", (ctx, cx, cy) => {
    const radius = 1; //dw-unit
    const angle = Math.atan2(dw.c.dy, dw.c.dx);
    const lookAtX = Math.cos(angle) * radius + dw.c.x;
    const lookAtY = Math.sin(angle) * radius + dw.c.y;
    ctx.lineWidth = 4;
    ctx.strokeStyle = "red"
    ctx.beginPath();
    ctx.moveTo(dw.toCanvasX(cx), dw.toCanvasY(cy));
    ctx.lineTo(dw.toCanvasX(lookAtX), dw.toCanvasY(lookAtY));
    ctx.stroke();
});
```

This will draw a red line in a radius of 1 dw-unit in the direction your character is looking at.

If you want to learn more about what's possible with the Canvas2D API, you can check out the  
[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D).
