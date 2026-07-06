# Mapmaker guide: Raid Assistant

Documentation for using **Raid Assistant** in adventure maps and custom worlds. These guides cover the **raid zombie** mob (GeckoLib models, scoreboard-driven animation, summon NBT), the **raid animated item** (first-person GeckoLib animations), optional **player key binds**, and an optional **player air dash**. No Java or modding experience required.

## Guides

| Guide | What it covers |
| ----- | -------------- |
| [Getting started](getting-started.md) | Fabric setup, version compatibility, resource packs, end-to-end checklist |
| [Raid zombie](raid-zombie.md) | Asset paths, variants, summon NBT, hitboxes, AI, animation control |
| [Raid animated item](raid-animated-item.md) | First-person item animations driven by player scoreboard |
| [Player air dash](air-dash.md) | Horizontal dash gamerule and scoreboard tuning |
| [Player key binds](key-binds.md) | Ability keys and click detection for map logic |
| [Quick reference](quick-reference.md) | IDs, objectives, and paths at a glance |
| [Troubleshooting](troubleshooting.md) | Common problems and fixes |

## What Raid Assistant adds

- A custom mob, the **raid zombie** (`raid-assistant:raid_zombie`), with 3D models and animations via **GeckoLib**.
- A custom item, the **raid animated item** (`raid-assistant:raid_animated_item`), with **first-person-only** GeckoLib animations driven by a **player** scoreboard (see [Raid animated item](raid-animated-item.md)).
- Extra **key binds** under *Raid Bindings* in Controls, which notify the server so maps can react via stats (see [Player key binds](key-binds.md)).
- An optional **horizontal air dash** for players, toggled with gamerules (see [Player air dash](air-dash.md)).

For raid zombies you will:

- Run **Minecraft Java Edition** with **Fabric** and this mod (plus dependencies as listed in [Getting started](getting-started.md#game-and-mod-platform)).
- Optionally (but recommended) ship a **resource pack** that overrides or adds models, textures, and animation JSON under a fixed folder layout.
- Use **commands** (and often **cheats** or operator permissions) to summon mobs and drive poses with a **scoreboard**.

For the raid animated item you will:

- Give players `raid-assistant:raid_animated_item` and drive first-person poses with a **player** scoreboard (see [Raid animated item](raid-animated-item.md)).
- Optionally ship item GeckoLib assets under `geo/item/`, `textures/item/`, and `animations/item/` (same stem rules as raid zombies).
- Players need the mod on the **client** to see first-person animations; other players do not see them.