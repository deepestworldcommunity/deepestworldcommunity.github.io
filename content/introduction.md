---
title: "Introduction"
date: 2023-09-03T00:00:00+02:00
draft: false
bookToc: true
---
# Introduction

[Deepest World](https://deepestworld.com) is a browser-based sandbox MMORPG where you can code your character with JavaScript.
You can also take a look at the official [API documentation](https://deepestworld.com/api).

## The World

Besides the first impression the world has indeed three dimensions: `x`, `y`, `z`. 
This will always be shown in the top right corner of the game.
The world is in theory endless.
Every location in the game has a zone level associated (also shown in the top right) based on the coordinates, 
that can also be calculated via the in-game function `dw.getZoneLevel()`:

```js
dw.getZoneLevel = (x, y, z) => {
    const dx = (x ?? dw.c.x) / 16;
    const dy = (y ?? dw.c.y) / 16;
    const dz = (z ?? dw.c.z);

    return Math.floor(1 + Math.sqrt(dx * dx + dy * dy + dz * dz));
};
```

## Character

In that world you control a character (accessible via code as `dw.character`) 
that has health points `dw.character.hp` and mana points `dw.character.mp` to use skills.

Your Character starts at level 1 with 100 hp & 100 mp. 
When killing monsters similar to your characters level it gains experience points XP, 
also available via API as `dw.character.xp`. 
The higher your characters level the more XP will be needed to level up.

When killing enough monsters your character will automatically receive loot and put in its inventory `dw.character.bag`.
In-game you can access the inventory by pressing the key `I`. 
There are 40 inventory slots, but not all inventory slots are equal. 
The last row `dw.character.bag[32 - 39]` can be used to combine stackable items with different levels.

## Items

There are different types of items: 
* armor
* weapons
* accessories
* gems
* skills
* crafting materials
* stations
* boxes

Everything besides boxes has a level. This will be shown in the top left corner of the item.
Some items are stackable (up to 100), visible in the right bottom corner. 
There are different rarities indicated by different item border colors:

* common (white)
* uncommon (green)
* rare (blue)
* epic (purple)
* legendary (orange)

### Armor, Weapons & Accessories

When you press `C` in-game the character window appears.
There you can see all the character attributes, profession levels and gear slots.
Each item slot only allows to put an item of the dedicated item type into it.

Whether your character can equip a shield depends on the weapon type equipped - 
bows and crossbows require two hands and thus do not allow a shield to be equipped.
When equipping a shield your character gains the benefits from the shield, 
but also will automatically do a percentage less damage. 

### Gem Pyramid

Things become interesting at level 2. At this level and on every other levels your character 
gains will provide an extra gem slot. In time, it will form into a pyramid. You can access
it via pressing the `P` key.

### Skillbar

At the bottom of the screen is the skill bar. It behaves like a regular inventory slots
but the main purpose is to put skills there. Your character can cast them if it has enough
mp. There are 8 slots that can be used by the keys `1` - `8`.

## Custom Keybindings

You can change all the mentioned keybindings in the menu.
