# Player key binds

[← Back to guide index](README.md)

Raid Assistant adds rebindable keys under **Controls → Raid Bindings**. Pressing a bind sends an event to the **server**, where the mod updates **stats** you can hook with command blocks, functions, advancements, or datapacks.

Players need the mod on the **client** for these binds to work (unlike [air dash](air-dash.md), which is server-only).

## Ability keys

| Bind (default) | Controls label | Server effect on press                                      |
| -------------- | -------------- | ----------------------------------------------------------- |
| **R**          | Ability 1      | Increments custom stat `raid-assistant:ability1` by **1**.  |
| **V**          | Ability 2      | Increments custom stat `raid-assistant:ability2` by **1**.  |

Each key fires **once per press** (not while held).

Example: copy a player’s Ability 1 count into a scoreboard objective for command-block logic:

```mcfunction
scoreboard objectives add ra_ab1 dummy
execute as @a store result score @s ra_ab1 run data get entity @s Stats.custom:{"raid-assistant:ability1":0}
```

Run that on a clock or after you expect a press. You can use the same pattern for `raid-assistant:ability2`.

## Click detection (held item)

For items tagged `#raid-assistant:click_detection`, the mod reports left and right clicks to the server using **vanilla item stats** on the item in the player’s **main hand**:

| Input                         | When it counts                         | Server effect                                              |
| ----------------------------- | -------------------------------------- | ---------------------------------------------------------- |
| **Attack** (left click)       | Item in main hand has the tag          | Increments that item’s **times broken** stat by **1**.      |
| **Use** (right-click key)     | Item in main hand has the tag          | Increments that item’s **times used** stat by **1**.       |

Add items to the tag in a datapack, for example `data/raid-assistant/tags/item/click_detection.json`:

```json
{
  "values": [
    "minecraft:carrot_on_a_stick",
    "minecraft:stick"
  ]
}
```

**Note:** The use key is checked **every tick while held**, so holding right click can increment **times used** rapidly. Prefer short clicks, or design logic that tolerates repeated counts.

Left click only counts on an actual attack action (one press per swing).

See also: [Quick reference — Key binds](quick-reference.md#key-binds) and [Troubleshooting](troubleshooting.md).
