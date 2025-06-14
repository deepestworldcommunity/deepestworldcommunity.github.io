---
title: "Mods"
draft: false
---
# Mods

There are lots of mods on different items. Currently there are 4 different mods on each equipment item.  
The mod values of the item you inspect, aren't that, what you want. There is a dw-function that will show you the correct values of "modName", which you can see in tooltip too.  
- `dw.itemModValue(item, section, modName)` returns number.
  - `item`: item you can equip, either from `dw.c.gear[md]`, your inventory or bank  
  - `section`: one of "baseMods", "cmods", "imods" or "mods"  
  - `modName`: "hp", "strDef", "physDmg" and more similar

Here is an example of code, that shows you all real values you can get from item.
```js
//made by Hunsrak
function checkMods(item) {
    let result = {}
    const mods = ["baseMods","cmods","imods","mods"]
    for (const mod of mods) {
        if(Object.keys(item[mod]).length === 0) continue
        for (const key in item[mod]) {
            result[key] = dw.itemModValue(item, mod, key)
        }
    }
    
    if(Object.keys(result).length === 0) {console.log("There are no mod values to read"); return}
    console.log(item.name)
    for (const key in result) {
        console.log(`${key} : ${result[key]}`)
    }
}
```
