# Gathering Resources

The same process like finding monsters in Deepest World is done with gathering resources too, but different.
Let's find the closest resource that match the criteria in `dw.isGatherable(entity)`, which returns boolean.

```js
const resource = dw.findClosestEntity(e => dw.isGatherable(e));
if(resource && dw.canGather(resource)) {
  dw.gather(resource);
}
```

Or Simply look for trees with this function.

```js
dw.findClosestTree() // This will return the closest tree to your character
```

## find, move, gather and loop

After you learned how to find entities, attack or gather them and how to move around Deepest World, it's time to go a step further. We'll work with an example for chopping trees.

```js
function chopTree() {
  const tree = dw.findClosestTree();
  if(tree) {
    dw.move(tree.x, tree.y);
    if(dw.canGather(tree)) {
      //dw.stop;
      dw.gather(resource);
    }
  }
}
```

We want to execute this code multiple times, so let's add a loop.  
There are multiple ways to loop code which are explained in *boilerplates/advanced*  
But right now, we'll use `setInterval()` for looping in this example.

```js
setInterval(chopTree(), 250); //repeats executing chopTree() every 250 milliseconds
```

And that's it, Look how your bot is running from tree to tree and chopping them! Have fun coding!