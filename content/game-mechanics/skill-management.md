---
title: "Skill Management"
draft: false
---

# Skill Management

Your skill bar is accessible to `dw.c.skills`, an array filled with (skill) objects. Each object has `.name`, `.supportSkills` and more properties.

## Skill slots

Before you can add skills, make sure that you have unlocked skill slots. 
Unlocking slots requires skill slot points, which you get from leveling up your character. 
You can add or remove skill slots with:

- main skills:
  - `dw.addSkillSlot()`
  - `dw.removeSkillSlot()` removes the last main skill slot. Note: first main skill slot can not be removed)  
- support skills:
  - `dw.addSupportSkillSlot(skillIndex)`
  - `dw.removeSupportSkillSlot(skillIndex)` removes the last support skill slot from corresponding main skill

When removing skill slots, you will get back all points you used on.

- cost to unlock new main skill slot: `dw.c.skills.length+1`  
- cost to unlock new support skill slot: `dw.c.skills[skillIndex].supportSkills.length+1`

To know how many slot points available you have, substract from `dw.c.lvl` the sum of all costs for used skill slot points in main skills and their support skills.  
Here is an example:

```js
function getAvailableSlotPoints() {
    let UsedPoints = 0
    for (let i=0; i < dw.c.skills.length; i++) {
        UsedPoints += i+1
        for (let j=0; j < dw.c.skills[i].supportSkills.length; j++) {
            UsedPoints += j+1
        }
    }
    console.log("Sum of used slot points:", UsedPoints)
    console.log("Available slot points:", dw.c.lvl-UsedPoints)
    return dw.c.lvl - UsedPoints
}
```

## Main skills

All learned main skills, can be found in `dw.c.learnedSkills` and added to skill bar.  
To add a main skill in your skill bar, use `dw.addSkill(skillIndex, skillName)`.  

**Parameters:**  
- `skillIndex` refers to `dw.c.skills[skillIndex]` at which index you want to add the main skill.  
- `skillName` is one of `.name` you can find in `dw.c.learnedSkills`  

## Support skills

All learned support skills are accessible to `dw.c.learnedSupportSkills`, which can be linked on your main skills.  
`dw.addSupportSkill(skillIndex, supportSkillIndex, supportSkillName)` adds support skill on corresponding main skill.  

**Parameters:**  
- `skillIndex` refers to `dw.c.skills[skillIndex]`.
- `supportSkillIndex` refers to `dw.c.skills[skillIndex].supportSkills[supportSkillIndex]`
- `supportSkillName` is one of `.name` you can find in `dw.c.learnedSupportSkills`

Note: Not all support skills can be added on every main skill.

## passive skills (passive stats)

Your learned passive skills are accessible to `dw.c.learnedPassiveSkills`.  
`dw.addPassiveSkillPoint(passiveSkillName, numPoints)` adds points to corresponding passive skill.  

**Parameters:**  
- `passiveSkillName` is one of `.name` you can find in `dw.c.learnedPassiveSkills`  
- `numPoints` number of points `.pts` you want to spend or take back. It can be a negative number too, to take back used points.

Examples:  
`dw.addPassiveSkillPoint("hpInc", -2)` takes back 2 points from `.hpInc`  
`dw.addPassiveSkillPoint("hpRegenInc", 3)` increases 3 `.pts` on `.hpRegenInc`

To know how many passive points left you have, you can substract the sum of all spend points 
`.pts` on your learned passives from `dw.c.lvl-1`.
