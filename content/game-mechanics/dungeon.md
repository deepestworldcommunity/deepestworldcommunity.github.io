---
title: "Dungeon"
draft: false
---
# Dungeon

The Dungeon is an underground area, where you can mine ores and loot items. 
You find the Dungeon in underground by entering the portal at coordinates (0,0,0), which teleports you to a room named simulation. 
In center of simulation room you'll find the mainframe.
The mainframe allows you to create `dw.createSim(simItemIndex)`, enter `dw.enterSim(id)` or abandon `dw.abandonSim(id)` dungeons (sims).

### Create new sim

Before you can create dungeons, first you need to craft a sim item, which requires one essence at any level and rarity you have.

```js
const simItem = {
    profession: dw.enums.Profession.NONE,
    type: dw.enums.Type.SIM,
    lvl: dw.character.level,
    num: 1,
    r: dw.enums.Rarity.GREEN
}
//craft one green sim item at your character level
await dw.craft(simItem)
```

Put sim item in the mainframe slot and activate it. Now you can see, that a new dungeon appeared in your list.  
You access them with `dw.c.sims` (array of (dungeon) objects or empty array).  
Inside (dungeon) object you can see `.id`, `.progress` and more properties.

### What's the goal?

After you entered the dungeon you'll be teleported in your newly created dungeon.  
You finished the dungeon, if you reach progress at 100%. The progress increases whenever you kill a dungeon monster. Once you reach 99% of progress, a boss will spawn somewhere in your dungeon. Find and beat the boss, great rewards await you!  
If you want to leave your dungeon, you can always `dw.exitSim(id)`.

### What makes sim/dungeon special?

1. Drop rates for items are higher than in the overworld.
2. It is the only area where you can mine ores.
3. A great place to improve your pathfinding or movement/exploration algorithms. Sometimes, the dungeon map is build like a labyrinth.
