---
title: "Skill Management"
draft: false
---

# Skill Management

Your skill bar is accessible to `dw.character.skills`, which returns an array with (skill) objects. 
Each (skill) object has `.name`, `.supportSkills` and more properties.  
`.name` is a string (name of main skill).  
`.supportSkills` is an array of (linked support skill) objects.

## Skill slots

Before you can add skills, make sure, that you have unlocked skill slots available.  
Unlocking slots requires skill slot points, which you get from leveling up your character.  
- main skills:
  - `dw.addSkillSlot()`
  - `dw.removeSkillSlot()` removes the last main skill slot.  
    - Note: first main skill slot can not be removed.
- support skills:
  - `dw.addSupportSkillSlot(skillIndex)`
  - `dw.removeSupportSkillSlot(skillIndex)` removes the last support skill slot from corresponding main skill  

Parameter `skillIndex` refers to `dw.c.skills[skillIndex]`

When removing skill slots, you will get back all slot points you used on before then.

- cost to unlock new main skill slot: `dw.character.skills.length+1`  
- cost to unlock new support skill slot: `dw.character.skills[skillIndex].supportSkills.length+1`

To know how many slot points available you have, substract from `dw.character.lvl` the sum of all costs for used skill slot points in main skills and their support skills.  
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

All learned main skills, can be found in `dw.character.learnedSkills`, which can be added in your skill bar.  
`dw.addSkill(skillIndex, skillName)` adds main skill in your skill bar  
`dw.removeSkill(skillIndex)` removes main skill from your skill bar (including its support skills)

**Parameters:**  
- `skillIndex` refers to `dw.c.skills[skillIndex]`
- `skillName` is one of `.name` you can find in `dw.c.learnedSkills`  

## Support skills

All learned support skills are accessible to `dw.c.learnedSupportSkills`, which can be linked on your main skills.  
`dw.addSupportSkill(skillIndex, supportSkillIndex, supportSkillName)` adds support skill on corresponding main skill.  
`dw.removeSupportSkill(skillIndex, supportSkillIndex)` removes support skill from corresponding main skill.

**Parameters:**  
- `skillIndex` refers to `dw.c.skills[skillIndex]`.
- `supportSkillIndex` refers to `dw.c.skills[skillIndex].supportSkills[supportSkillIndex]`
- `supportSkillName` is one of `.name` you can find in `dw.c.learnedSupportSkills`

Note: Not all support skills can be added on every main skill.

## Passive skills (passive stats)

Your learned passive skills are accessible to `dw.c.learnedPassiveSkills`.  
`dw.addPassiveSkillPoint(passiveSkillName, numPoints)` adds points to corresponding passive skill.  

**Parameters:**  
- `passiveSkillName` is one of `.name` you can find in `dw.c.learnedPassiveSkills`  
- `numPoints` number of points `.pts` you want to spend or take back. It can be a negative number too, to take back used points.
  - Note: if you take back points, you'll have to pay some silver according to respec cost

Examples:  
`dw.addPassiveSkillPoint("hpInc", -2)` takes back 2 `.pts` from `.hpInc`  
`dw.addPassiveSkillPoint("hpRegenInc", 3)` spends 3 `.pts` on `.hpRegenInc`

To know how many passive points left you have, you can substract the sum of all spend points on your learned passives from `dw.c.lvl-1`.
