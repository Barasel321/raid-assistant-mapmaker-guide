# Raid zombie

[← Back to guide index](README.md)

The **raid zombie** (`raid-assistant:raid_zombie`) is a GeckoLib mob with custom models per summon, scoreboard-driven animation, configurable hitboxes, and AI presets.

## Asset paths

All raid zombie **GeckoLib assets** for this mod live under the namespace `raid-assistant`. You cannot change that namespace in a resource pack and still have this mob load those files.

Folder layout inside your pack (or inside the mod jar for defaults):

```text
assets/raid-assistant/
  geo/entity/<stem>.geo.json
  textures/entity/<stem>.png
  animations/entity/<stem>.animation.json
```

- `<stem>` is a short name you choose for one “look” (for example `raid_zombie`, `boss_zombie`).
- The **same** stem must be used for all three files for that look. The mod does not support using one model stem with a different texture stem.

## Default content and variants

### Default stem: `raid_zombie`

The mod ships default files:

- `geo/entity/raid_zombie.geo.json`
- `textures/entity/raid_zombie.png`
- `animations/entity/raid_zombie.animation.json`

If you do not set `RaidVariant` on summon, or you set an invalid value, the mob uses this stem.

### Adding a new look (variant)

1. In Blockbench (with GeckoLib export), create a model and animations compatible with GeckoLib’s **entity** layout.
2. Export (or copy) three assets that share one stem, for example `my_boss`:
   - `assets/raid-assistant/geo/entity/my_boss.geo.json`
   - `assets/raid-assistant/textures/entity/my_boss.png`
   - `assets/raid-assistant/animations/entity/my_boss.animation.json`
3. **Bone names and animation structure** must agree between the geo and animation files (standard GeckoLib / Blockbench workflow). If they disagree, you may see a broken pose or errors in the log—not a mapmaking topic this doc can debug in depth.
4. Enable your resource pack **above** or in a sensible order relative to the mod so your files override or supplement as you intend.

## Spawning and NBT

### Entity type id

Always summon:

```text
raid-assistant:raid_zombie
```

### Visual variant (`RaidVariant`)

| Property               | Value                                                                      |
| ---------------------- | -------------------------------------------------------------------------- |
| **Key**                | `RaidVariant`                                                              |
| **Type**               | String                                                                     |
| **Meaning**            | Chooses the **stem** for all three asset paths above.                      |
| **Allowed characters** | Lowercase letters, digits, underscore, hyphen only: `a-z`, `0-9`, `_`, `-` |
| **Max length**         | 64 characters                                                              |
| **Invalid or missing** | Treated as stem `raid_zombie`                                              |

Example summon:

```mcfunction
summon raid-assistant:raid_zombie ~ ~ ~ {RaidVariant:"my_boss"}
```

You can combine tags on one summon, for example a silent mob with passive AI (see [AI preset](#ai-preset-raidai)):

```mcfunction
summon raid-assistant:raid_zombie ~ ~ ~ {RaidVariant:"my_boss",RaidAI:"wanderer",Silent:1b}
```

With custom hitbox size:

```mcfunction
summon raid-assistant:raid_zombie ~ ~ ~ {RaidVariant:"my_boss",RaidHW:1.0f,RaidHH:2.5f}
```

### Hitbox width and height (`RaidHW`, `RaidHH`)

Minecraft’s entity hitbox is a vertical box with **one horizontal width** used for **both X and Z**. There is **no separate depth** in this implementation.

Keys are short so they are easy to type in commands: `RaidHW` (hitbox width) and `RaidHH` (hitbox height).

| NBT key  | Type            | Meaning                     | Default (blocks) | Allowed range (clamped) |
| -------- | --------------- | --------------------------- | ---------------- | ----------------------- |
| `RaidHW` | Float or double | Horizontal extent (X and Z) | `0.6`            | `0.2`–`10.0`            |
| `RaidHH` | Float or double | Vertical extent             | `1.95`           | `0.5`–`15.0`            |

You may set **one** or **both** keys on summon; omitted keys keep the default for that axis. Values are **synced to clients** and **saved** with the entity (structures, world, `/data`).

The mod applies these sizes by scaling the mob’s **base** dimensions for each pose (standing, swimming, etc.). Vanilla’s `generic.scale` attribute and related logic still apply **on top** of that, same as other living entities.

Example:

```mcfunction
summon raid-assistant:raid_zombie ~ ~ ~ {RaidHW:1.2f,RaidHH:3.0f}
```

#### Limitations and caveats

- **No independent “depth”:** You cannot make the box wider on X than on Z through these tags; both axes share `RaidHW`.
- **Model vs hitbox:** GeckoLib **does not** resize the visible model to match the hitbox. A huge hitbox with a tiny model (or the opposite) is allowed; players may find hits and visuals mismatched until you adjust the Blockbench model scale or hitbox together.
- **Pose scaling:** Dimensions are applied as a **scale** on top of vanilla zombie dimensions for each **pose** (standing, swimming, baby, etc.). Unusual combinations (for example **baby** raid zombies) may not match a plain “adult box” mental model.
- **Reach and combat:** Attack range and “where the mob is” for the server still follow vanilla zombie-style logic; extreme sizes can feel odd for melee.
- **Clamping:** Values outside the table are **clamped**, not errors. `NaN` / infinity fall back to defaults.

### AI preset (`RaidAI`)

Use `RaidAI` to pick one of the mod’s built-in behavior presets. This is separate from vanilla `NoAI`: `NoAI:1b` disables **all** AI (including wander), while `RaidAI:"wanderer"` keeps passive movement.

| Property               | Value                                                                 |
| ---------------------- | --------------------------------------------------------------------- |
| **Key**                | `RaidAI`                                                              |
| **Type**               | String                                                                |
| **`hostile`** (default) | Chase and melee players when in range; wander and look at players when idle. |
| **`wanderer`**         | Swim and wander only; no targeting, melee, or look-at goals.          |
| **Invalid or missing** | Treated as `hostile`                                                  |

Example passive NPC:

```mcfunction
summon raid-assistant:raid_zombie ~ ~ ~ {RaidVariant:"my_boss",RaidAI:"wanderer"}
```

`RaidAI` is **synced**, **saved** with the entity (structures, world, `/data`), and can be changed later with `/data merge entity`.

### Persistence

Custom NBT on raid zombies (`RaidVariant`, `RaidHW`, `RaidHH`, `RaidAI`) is stored on the entity like other mob data: **structure blocks**, **world save**, and `/data` can preserve it. Re-loading the world keeps those values as long as the entity is saved correctly.

### Missing pack files (no crash)

If `RaidVariant` names a stem that passes the rules but **there is no** `geo/entity/<stem>.geo.json` in the loaded resources, the **client** falls back to the default `raid_zombie` model, texture, and animation file so the game does not crash. Scoreboard-driven animation order then uses the **default** animation file as well until the geo exists.

You should still ship all three files for a polished map.

## Animation control

The mob does **not** pick animation clips by a fixed name you hard-code in commands. Instead, the server reads a **scoreboard** value and the client plays clips by **order** in the JSON file for that mob’s **effective** stem (after any fallback).

### Scoreboard objective

Create a **dummy** objective with this exact name:

```text
ra_rz_anim
```

Example:

```mcfunction
scoreboard objectives add ra_rz_anim dummy
```

If this objective **does not exist**, the mob behaves as if the score were **0** (frozen pose for scripted clips).

### Score meaning

| Score | Behavior                                                                                       |
| ----- | ---------------------------------------------------------------------------------------------- |
| **0** | No looping clip from the ordered list (freeze / idle handling depends on your animation file). |
| **1** | First animation entry (see below).                                                             |
| **2** | Second animation entry.                                                                        |
| **3** | Third, and so on.                                                                              |

The mod reads the score **for that entity** on the **server** each tick and syncs the index to the client.

Example: set the nearest raid zombie’s score to 2:

```mcfunction
scoreboard players set @e[type=raid-assistant:raid_zombie,sort=nearest,limit=1] ra_rz_anim 2
```

### Order matters, names do not

In `<stem>.animation.json`, GeckoLib uses a top-level `animations` object. The mod builds an ordered list from the **keys** of child entries that are **JSON objects**, in **declaration order** (the order they appear in the file).

- **Mapmakers:** score `1` = first such entry, `2` = second, etc.
- **Artists:** clip **names** can be anything Blockbench exports; rename freely as long as **order** in the file matches what your map expects.

Only entries whose value is a **JSON object** count in that list (the same filtering rule the mod has always used).

After editing animation JSON, press **F3+T** to reload resources (see [Getting started — Resource reload](getting-started.md#resource-reload-and-caching)).
