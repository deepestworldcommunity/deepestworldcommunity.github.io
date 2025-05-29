---
title: "Mods"
draft: false
---
# Mods

There are lots of mods on different items. Currently there are 4 different mods on each equipment item.  
The mod values of the item you inspect aren't right. So there is a function that will show you the correct value of modName, which you can see in tooltip too.  
`dw.itemModValue(item, section, modName)` Returns number
- `item`: item you can equip, one of `dw.c.gear[md]`, in your inventory or bank
- `section`: one of "baseMods", "cmods", "imods" or "mods"  
- `modName`: "hp","strDef", "physDmg" and more similar

Hunsrak made a cool algorithm, that shows you all real values you can get from that one item.
```js
function checkMods(item) {
    console.log(item.name)
    let result = {}
    const mods = ["baseMods","cmods","imods","mods"]
    for (const mod of mods) {
        if(Object.keys(item[mod]).length === 0) continue
        for (const key in item[mod]) {
            const value = dw.itemModValue(item, mod, key)
            result[key] = value
        }
    }
    
    if(Object.keys(result).length === 0) {console.log("There are no mod values to read"); return}
    for (const key in result) {
        console.log(`${key} : ${result[key]}`)
    }
}
```
