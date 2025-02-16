# Gathering Resources

Similar to finding monsters you can find resources in Deepest World. 
There is a helper function called `dw.isGatherable(entity)` which returns a boolean if the entity can be gathered.
There also is a helper function that is looking only for trees called `dw.findClosestTree(filter)`.

```js
const resource = dw.entities.find(e => dw.isGatherable(e));
const closestResource = dw.findClosestEntity(e => dw.isGatherable(e));
const closestTree = dw.findClosestTree();
```

Besides trees there are also plants and ores. On the surface ores will only be stones. To find metal ores you will have to go underground.
Gathering can be done via calling `dw.gather(target)`, but additional checks should to be made:

* `dw.isReady()` checks the global cooldown, so whether you can currently gather
* `dw.isInRange(skillIndex, target)` checks if you are in range to gather, 
  as long as you pass a resource as the target, the skillIndex gets ignore,
  the API docs suggests to use `null` here

```js
const resource = dw.findClosestEntity(e => dw.isGatherable(e));
if (resource && dw.isReady() && dw.isInRange(null,resource)) {
  dw.gather(resource);
}
```

## Find, Move, Gather And Loop

A simple gathering loop can look like this:

```js
function gatherResources() {
  const closestResource = dw.findClosestEntity(e => dw.isGatherable(e));
  if (!closestResource) {
    // TODO: add some exploration code here
    return;
  }
  
  if (!dw.isInRange(null, closestResource)) {
    dw.move(closestResource.x, closestResource.y);
    return;
  }
  
  if (!dw.isReady()) {
    return;
  }
  
  dw.gather(closestResource);
}

setInterval(gatherResources, 250); //repeats executing chopTree() every 250 milliseconds
```

So every 250 milliseconds the bot will look for the closest resource, move to it, gather it and repeat.
And that's it, look how your bot is running from tree to tree and chopping them! Have fun coding!
You will quickly see that the bot is just waiting for a resource to respawn and not doing anything in the meantime.
You can add some [exploration code](/tutorial/exploration) to the bot to make it more efficient.
