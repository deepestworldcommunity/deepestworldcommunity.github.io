---
title: "Battle Score"
draft: false
weight: 5
---
# Battle Score

Battle Score is an attempt to evaluate the power level of your character and monsters.
It comes from a simple idea that your character should only attack monsters 
that it can kill faster than being killed.

| number of rounds to kill the monster | < | number of rounds to kill the character |
|-------------------------------------:|:-:|:---------------------------------------|
|        monster.hp / character.damage | < | character.hp / monster.damage          |
|          monster.hp * monster.damage | < | character.hp * character.damage        |

In short **health points \* damage** is considered to be the BattleScore.

Getting the health points is easy, damage on the other hand is a different topic, there are many factors:
* base damage is between 80% - 120%
* critical hits
* pure 10% more dmg increase for physical damage
* debuffs
* buffs
* armor
* life regeneration
* parry
* multiple skills
* skill components
* line of sight
* range

But since this is meant ot be an easy attempt we simply use the base damage and at least include the 
critical hit part, since that one is easy to obtain. So it would look like this for the characters:

```js
function getCharacterBattleScore() {
  let maxDmg = 0
  dw.character.skills.forEach((skill) => {
    if (!skill || /heal|shield/.test(skill.md)) {
      return
    }

    let dmg = (skill?.phys ?? 0) 
        + (skill?.fire ?? 0) 
        + (skill.acid ?? 0) 
        + (skill.cold ?? 0) 
        + (skill.elec ?? 0)
    
    dmg *= 1 + (skill.crit ?? 0) * ((skill.critMult ?? 1) - 1)

    if (dmg > maxDmg) {
      maxDmg = dmg
    }
  })

  return Math.sqrt(maxDmg * dw.character.hpMax)
}
```

If you wonder why the square root at the end... if to keep the number relatively small, 
but big enough to be meaningful. At level 50 the Battle Score would be around ~42,250,000.
However, with the square root it's around ~6,500.

For monsters this would look like this:

```js
const BOSSES = [
    'kingGoo',        // Event Boss: spawned by killing goo
    'kingSpikedGoo',  // Event Boss: spawned by killing goo
    'alarmBoss',      // Event Boss: spawned by killing alarmOnDeath
    'bouncyGoo',      // Event Boss: spawned by killing goo
    'sdBoss',         // Dungeon Boss: Squirrel Den
]

function getMonsterBattleScore(monster: DeepestWorld.Monster) {
    let dmg = 19 * Math.pow(1.1, monster.level)

    // Factor in critical hits
    dmg += 1 + 0.05 * 0.5

    const skullData = monster.fx.skulls
    let skullCount = 0
    if (
        skullData &&
        typeof skullData === 'object' &&
        's' in skullData &&
        typeof skullData.s === 'number'
    ) {
        skullCount = skullData.s
    }

    // Scale based on skulls on mob, extra 25% for bosses
    dmg *= 1
        + skullCount * 0.5
        + (BOSSES.includes(monster.md) ? 0.25 : 0)

    // Powerful mobs deal 25% more dmg
    if (monster.fx.dmgMore) {
        dmg *= 1.25
    }

    // Quick mobs attack 20% more often, thus deal more dmg
    if (monster.fx.quick) {
        dmg *= 1.2
    }

    return Math.sqrt(dmg * monster.hpMax)
}
```
