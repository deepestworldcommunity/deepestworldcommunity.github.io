---
title: "2 Getting Started"
draft: false
---
# Getting Started

Your character starts at level 1 without any equipment besides an `attackRune`.
An `attackRune` is a skill rune that's already in your skill bar slot #1 and uses your 
weapon or in the case of the starting character an unarmed strike.

Running your character via code requires it to regularly check for possible actions and 
choose one of them. Running a busy loop in JavaScript would block the browser, so having 
non-blocking code is easiest done with `setInverval()` or `setTimeout()` but you could 
use a loop & async/await as well if you prefer that.

An example code could like that:

```js
setInterval(function () {
  // Search for the closest monster
  const target = dw.findClosestMonster()
  if (!target) {
    // No target found
    return
  }

  // Show target in game UI
  dw.setTarget(target.id)
    
  // Find an appropriate skill rune
  const skillIndex = dw.character.skills.findIndex(
    (skill) => skill && skill.md === 'attackRune'
  )
  if (skillIndex === -1) {
    // No attackRune found
    return
  }

  if (!dw.isSkillInRange(skillIndex, target.x, target.y)) {
    // Too far away
    dw.move(target.x, target.y)
    return
  }

  if (!dw.isSkillReady(skillIndex)) {
    // Skill is on cooldown
    return
  }

  // Use the skill
  dw.useSkill(skillIndex, target.id)
}, 250)
```

It periodically:
* looks for the closest monster
* sets the target in the UI
* selects the skill
* checks whether the target is in range
* moves closer if it isn't
* and finally uses the skill on the monster

---

If you run that code for a while you'll notice obvious issues:
* the character just attacks any monster it sees
* the character doesn't seem to care how injured it already is and persists attacking
* if nothing is in sight the character just stands around and does nothing
* if the closest monster is behind a wall or tree the character gets stuck

You can work on any of those issues. Additional suggestions would be:
* separate moving & attacking into different loops
* add moving skills into the mix
* move from `setInverval` to `setTimeout`, especially for the skills bound to the global cooldown

But you can also look into improving your gear and ignore that for the moment.

## Improving Gear

If you haven't found a weapon or an armor piece for each slot you could start chopping (right click) some trees. 
After you chopped some trees you can put one of the wood to the ground. Right click on it in the inventory 
and place it into the world  you might have to get farther away from the spawning spot at 0,0,0 to do so. 
When the wood  is placed down you can use it to craft a workbench. 
When you put that one down as well you  can craft a weapon of your choosing and an armor piece 
(helmet, chest-plate, gloves, boots) for every empty slot.

Besides the craftable, mostly common (white) items, you should probably look for uncommon (green) items.
Anything with `+X Life` will let your character last longer in fights, but also anything that gives you 
any kind of flat damage will also help you kill things faster. More information on that can be found in 
the [Battle Score](/battle-score) section. 
