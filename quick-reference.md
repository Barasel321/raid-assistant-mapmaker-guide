# Quick reference

[← Back to guide index](README.md)

## Raid zombie

| Topic                | Value                                                                  |
| -------------------- | ---------------------------------------------------------------------- |
| Entity id            | `raid-assistant:raid_zombie`                                           |
| Variant NBT          | `RaidVariant` (string stem)                                            |
| Hitbox NBT           | `RaidHW` (width), `RaidHH` (height); float or double (see [Raid zombie](raid-zombie.md#hitbox-width-and-height-raidhw-raidhh)) |
| AI preset NBT        | `RaidAI` — `hostile` (default) or `wanderer` (see [Raid zombie](raid-zombie.md#ai-preset-raidai)) |
| Default stem         | `raid_zombie`                                                          |
| Default hitbox       | width `0.6`, height `1.95` blocks (clamp: `0.2`–`10.0` / `0.5`–`15.0`) |
| Scoreboard objective | `ra_rz_anim` (dummy)                                                   |
| Namespace in pack    | `raid-assistant`                                                       |
| Geo path             | `assets/raid-assistant/geo/entity/<stem>.geo.json`                     |
| Texture path         | `assets/raid-assistant/textures/entity/<stem>.png`                     |
| Animation path       | `assets/raid-assistant/animations/entity/<stem>.animation.json`        |

## Raid animated item

| Topic                | Value                                                                  |
| -------------------- | ---------------------------------------------------------------------- |
| Item id              | `raid-assistant:raid_animated_item`                                    |
| Variant custom data  | `RaidVariant` (string stem) on `/give`                                 |
| Default stem         | `raid_item`                                                            |
| Scoreboard objective | `ra_item_anim` (dummy), holder = **player**                            |
| Animation scope      | First person, main hand, local player only                             |
| Item model path      | `assets/raid-assistant/models/item/raid_animated_item.json` (`builtin/entity`; one file for the item id, not per stem) |
| Geo path             | `assets/raid-assistant/geo/item/<stem>.geo.json`                       |
| Texture path         | `assets/raid-assistant/textures/item/<stem>.png`                       |
| Animation path       | `assets/raid-assistant/animations/item/<stem>.animation.json`          |
| Client mod           | **Required** for animated first-person view                            |

## Player air dash

| Topic            | Value                                              |
| ---------------- | -------------------------------------------------- |
| Enable           | `/gamerule allowRaidDash true`                     |
| Cooldown (ticks) | Scoreboard `ra_dash_cd` on **player** (0 = none)   |
| Default cooldown | `20` ticks if objective missing (~1 s at 20 TPS) |
| Strength         | Scoreboard `ra_dash_str` on **player** (hundredths; `75` = default speed) |
| Default strength | `75` (0.75 blocks/tick) if objective missing       |
| Trigger          | New sneak press while airborne, facing horizontally |

Full guide: [Player air dash](air-dash.md)

## Key binds

| Topic              | Value                                                                 |
| ------------------ | --------------------------------------------------------------------- |
| Controls category  | **Raid Bindings**                                                     |
| Ability 1 (default) | **R** → stat `raid-assistant:ability1`                               |
| Ability 2 (default) | **V** → stat `raid-assistant:ability2`                               |
| Clickable items    | Item tag `#raid-assistant:click_detection` in main hand               |
| Left click effect  | +1 to item **times broken** stat                                      |
| Right click effect | +1 to item **times used** stat (fires every tick while use is held)   |
| Client mod         | **Required** on players who use these binds                           |

Full guide: [Player key binds](key-binds.md)
