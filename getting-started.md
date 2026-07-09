# Getting started

[← Back to guide index](README.md)

## Game and mod platform

| Requirement                   | Notes                                                                                                              |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Minecraft Java Edition**    | Version **1.21.1** or **1.21.3**, depending on which Raid Assistant jar you install (see `fabric.mod.json` in the mod jar or repo). Jars are **not** interchangeable. |
| **Fabric Loader**             | Install via [fabricmc.net](https://fabricmc.net/).                                                                 |
| **Fabric API**                | Required dependency; place in your `mods` folder with the mod.                                                     |
| **GeckoLib** (Fabric)         | Required. Use the GeckoLib version that matches your Minecraft version (see [version compatibility](#version-compatibility) below). |
| **Raid Assistant** mod jar    | Your `raid-assistant` build or release, in `mods`.                                                                 |

Single-player “Open to LAN” with cheats, or a server where you have **operator** rights, is enough to use the commands in this guide.

## Version compatibility

Use the row that matches your Minecraft version. Mixing versions (for example a 1.21.1 mod jar on 1.21.3) will not work.

| Topic | 1.21.1 build | 1.21.3 build |
| ----- | ------------ | ------------ |
| **Raid Assistant jar** | 1.21.1 release | 1.21.3 release |
| **GeckoLib** | 4.8+ | 4.7.1+ |
| **Resource pack `pack_format`** | 34 | 42 |
| **Install checklist** | [1.21.1 checklist](#install-checklist-1211) | [1.21.3 checklist](#install-checklist-1213) |

Jars are **not cross-compatible** between 1.21.1 and 1.21.3. Mapmakers upgrading a world should install the matching mod, GeckoLib, and update resource pack `pack_format` if needed.

## Skills and tools (for custom visuals)

You only need these if you want **your own** model and animations instead of the mod’s default look.

| Tool / skill                       | Why                                                                                                                                                                                                               |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Resource packs**                 | Know how to make a folder, add `pack.mcmeta`, zip it or use a folder pack, and enable it in the game’s resource pack screen.                                                                                      |
| **Blockbench** (recommended)       | 3D modeling and animation for Bedrock-style / GeckoLib exports.                                                                                                                                                   |
| **GeckoLib plugin for Blockbench** | Exports `.geo.json`, `.animation.json`, and textures in layouts GeckoLib expects. Install from Blockbench’s plugin page; follow [GeckoLib’s wiki](https://github.com/bernie-g/geckolib/wiki) for export settings. |

If you only use the **default** `raid_zombie` look bundled in the mod, you can skip Blockbench and still use summon NBT and scoreboards.

## Resource pack format version

Your `pack.mcmeta` must use a `pack_format` valid for your Minecraft version. For **1.21.1**, packs commonly use format **34**; for **1.21.3**, use format **42**. If the game rejects the pack, check the official Minecraft wiki “Resource pack” page for the exact number for your minor version.

Minimal `pack.mcmeta` shape (1.21.1 example):

```json
{
  "pack": {
    "pack_format": 34,
    "description": "My raid zombie overrides"
  }
}
```

For **1.21.3**, use `"pack_format": 42` instead.

## Resource reload and caching

- After you edit **animation** JSON files, run a **resource reload** in vanilla: **F3+T** (hold F3 and press T). That refreshes the client’s list of animation clips for every `raid-assistant` file under `animations/entity/` and `animations/item/`.
- Adding or removing **geo** or **textures** also requires the pack to be loaded; reload or re-enter the world if the game does not pick up changes.

## End-to-end checklist (new variant `my_boss`)

### Install checklist (1.21.1)

1. Install **Fabric**, **Fabric API**, **GeckoLib 4.8+**, and the **Raid Assistant 1.21.1** jar.
2. Create a resource pack with valid `pack.mcmeta` (`pack_format`: **34**).
3. Add the three files with the same stem:
   - `assets/raid-assistant/geo/entity/my_boss.geo.json`
   - `assets/raid-assistant/textures/entity/my_boss.png`
   - `assets/raid-assistant/animations/entity/my_boss.animation.json`
4. Enable the pack in-game.
5. Run:
   `summon raid-assistant:raid_zombie ~ ~ ~ {RaidVariant:"my_boss"}`
6. Run:
   `scoreboard objectives add ra_rz_anim dummy`  
   then set scores on entities as needed.
7. If you change animation JSON, press **F3+T**.
8. (Optional) Set hitbox on summon, e.g. `{RaidHW:1.0f,RaidHH:2.5f}` (see [Hitbox width and height](raid-zombie.md#hitbox-width-and-height-raidhw-raidhh)), or AI preset `{RaidAI:"wanderer"}` (see [AI preset](raid-zombie.md#ai-preset-raidai)).

### Install checklist (1.21.3)

1. Install **Fabric**, **Fabric API**, **GeckoLib 4.7.1+**, and the **Raid Assistant 1.21.3** jar.
2. Create a resource pack with valid `pack.mcmeta` (`pack_format`: **42**).
3. Add the three files with the same stem:
   - `assets/raid-assistant/geo/entity/my_boss.geo.json`
   - `assets/raid-assistant/textures/entity/my_boss.png`
   - `assets/raid-assistant/animations/entity/my_boss.animation.json`
4. Enable the pack in-game.
5. Run:
   `summon raid-assistant:raid_zombie ~ ~ ~ {RaidVariant:"my_boss"}`
6. Run:
   `scoreboard objectives add ra_rz_anim dummy`  
   then set scores on entities as needed.
7. If you change animation JSON, press **F3+T**.
8. (Optional) Set hitbox on summon, e.g. `{RaidHW:1.0f,RaidHH:2.5f}` (see [Hitbox width and height](raid-zombie.md#hitbox-width-and-height-raidhw-raidhh)), or AI preset `{RaidAI:"wanderer"}` (see [AI preset](raid-zombie.md#ai-preset-raidai)).

See also: [Raid zombie](raid-zombie.md) for full summon NBT and animation details.
