# EquipGen (Web Version)

EquipGen is a web-based tool for generating equipment configuration files for **BBase**.  
Built with [**SvelteKit**](https://kit.svelte.dev/), it provides a structured, visual way to create Garry’s Mod item definitions directly in the browser.

The tool can read exported **PAC3** configuration files and automatically populate item data such as model path, position, angles, and scale. Users can adjust these values manually, select attachment points, and apply optional behaviours like bone merging or seasonal restrictions.  
When ready, EquipGen outputs a formatted Lua configuration block that can be downloaded and pasted directly into a Garry’s Mod script.

---

### Example Output

```lua
BB.EquipableItemData[374] = {
    Slot = ITEM_EQUIP_SLOT_MISC,
    AltSlot = ITEM_EQUIP_SLOT_ARMS,
    Models = {
        {Model = "models/weapons/c_models/some-model.mdl", Bone = "ValveBiped.Bip01_R_Hand", Scale = 0.55, AngleOffset = Angle(11.424, -2.944, -167.497), PosOffset = Vector(5.951, -1.18, -3.794)},
    },
}
