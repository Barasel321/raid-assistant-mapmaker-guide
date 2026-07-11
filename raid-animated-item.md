# Raid animated item

[← Back to guide index](README.md)

The **raid animated item** is a held item with GeckoLib 3D visuals. Its animation pose is driven by a **player** scoreboard objective. Animations play **only in first person** for the **local player** while the item is in the **main hand** — other players do not see them.

First-person and third-person can use **different models**. Third-person stays **unanimated**; it loads a separate stem that defaults to `{firstPersonStem}_tp` (see [First-person vs third-person stems](#first-person-vs-third-person-stems)).

## Item id and giving

Item id:

```text
raid-assistant:raid_animated_item
```

Example:

```mcfunction
/give @s raid-assistant:raid_animated_item
```

Optional visual variant (same rules as raid zombie `RaidVariant`):

```mcfunction
/give @s raid-assistant:raid_animated_item[minecraft:custom_data={RaidVariant:"my_gun"}]
```

With an explicit third-person stem override:

```mcfunction
/give @s raid-assistant:raid_animated_item[minecraft:custom_data={RaidVariant:"my_gun",RaidVariantTP:"custom_tp_stem"}]
```

With a custom hotbar/inventory icon stem:

```mcfunction
/give @s raid-assistant:raid_animated_item[minecraft:custom_data={RaidVariant:"my_gun",RaidVariantGUI:"my_gun_icon"}]
```

To hide the item in hotbar, inventory, ground, and item frames (held views unchanged):

```mcfunction
/give @s raid-assistant:raid_animated_item[minecraft:custom_data={RaidVariant:"my_gun",RaidVariantGUI:"none"}]
```

With a crossbow-style hold pose:

```mcfunction
/give @s raid-assistant:raid_animated_item[minecraft:custom_data={RaidPose:"crossbow"}]
```

See [Raid zombie — Visual variant](raid-zombie.md#visual-variant-raidvariant) for stem character rules (`a-z`, `0-9`, `_`, `-`).

## Asset paths

All raid animated item **GeckoLib assets** for this mod live under the namespace `raid-assistant`. You cannot change that namespace in a resource pack and still have this item load those files.

Folder layout inside your pack (or inside the mod jar for defaults):

```text
assets/raid-assistant/
  models/item/raid_animated_item.json
  geo/item/<stem>.geo.json
  textures/item/<stem>.png
  animations/item/<stem>.animation.json
```

- Default **first-person** stem: `raid_item` (bundled in the mod).
- Default **third-person** stem: `raid_item_tp` (bundled in the mod).
- For each stem used in a perspective, ship matching `geo`, `textures`, and `animations` files under that stem name.
- Invalid or missing `RaidVariant` values fall back to `raid_item`. If geo is missing for a custom **first-person** stem, the client also falls back to `raid_item`.
- If geo is missing for a custom **third-person** stem, the client falls back to `raid_item_tp` (not the first-person stem).

### Vanilla item model (`models/item/`)

The mod ships one Minecraft item model file:

```text
assets/raid-assistant/models/item/raid_animated_item.json
```

It contains only:

```json
{
  "parent": "builtin/entity"
}
```

That tells the game to hand off rendering to GeckoLib instead of drawing a flat 2D item model. This path is tied to the **item id** (`raid_animated_item`), not to `<stem>` or `RaidVariant`.

When you add a new visual variant, you only add the GeckoLib files under `geo/item/`, `textures/item/`, and `animations/item/` — **not** extra files under `models/item/`. Leave the bundled model file alone unless you are deliberately replacing mod assets.

## First-person vs third-person vs GUI stems

| Perspective | Stem source | Example |
| ----------- | ----------- | ------- |
| **First person** (main hand) | `RaidVariant` | `my_gun` → `geo/item/my_gun.geo.json` |
| **Third person** (hands in F5 / other players’ view) | `RaidVariantTP` if set, else `{firstPersonStem}_tp` | `my_gun` → `my_gun_tp` |
| **Inventory / hotbar / ground / frames** | `RaidVariantGUI` if set, else first-person stem | `my_gun_icon` → `geo/item/my_gun_icon.geo.json` |
| Default first person | `raid_item` | |
| Default third person | `raid_item_tp` | |
| Hide in inventory / hotbar / ground / frames | `RaidVariantGUI:"none"` | No geo required; held FP/TP views unchanged |

- Animation (`ra_item_anim`) only affects the **first-person** model.
- Third-person clips are **not** driven by the scoreboard; the TP model stays static.
- GUI-group displays are **not** animated; they use the GUI stem (or the first-person stem when `RaidVariantGUI` is unset).
- If geo is missing for a custom **GUI** stem, the client falls back to the effective **first-person** stem (not `raid_item_tp`).
- Stem length is capped (64). If `{firstPersonStem}_tp` would be too long, the client uses `raid_item_tp`.

## Hold pose (`RaidPose`)

Optional item `custom_data` key that controls how the **player model's arms** hold the item (third person, other players). Does **not** change first-person GeckoLib geo or `ra_item_anim` playback.

```mcfunction
/give @s raid-assistant:raid_animated_item[minecraft:custom_data={RaidPose:"crossbow"}]
```

| `RaidPose` value | Player arm pose |
| ---------------- | --------------- |
| `item` (default) | Normal item hold |
| `crossbow` or `crossbow_hold` | Crossbow hold (two hands) |
| `crossbow_charge` | Crossbow charging |
| `bow` | Bow draw |
| `block` | Shield block |
| `spyglass` | Spyglass |
| `horn` | Goat horn |
| `brush` | Brush |
| `spear` | Spear / trident throw |

- Missing or invalid values fall back to `item`.
- Two-handed poses (`crossbow`, `bow`, etc.) position the offhand arm like vanilla crossbow/bow items.
- Requires the mod on the **client** (rendering only; no scoreboard or server logic).

## Scoreboard objective

Create a **dummy** objective with this **exact** name:

```text
ra_item_anim
```

Example:

```mcfunction
scoreboard objectives add ra_item_anim dummy
```

The score is read on the **player** (not an entity) on the **server** and synced to that player's client. You do **not** need `scoreboard objectives setdisplay` for animation to work. If the objective does not exist, the item behaves as if the score were **0** (frozen pose).

## Score meaning

| Score | Behavior |
| ----- | -------- |
| **0** | No looping clip from the ordered list (freeze). |
| **1** | First animation entry in `animations/item/<stem>.animation.json`. |
| **2** | Second animation entry. |
| **3** | Third, and so on. |

Example:

```mcfunction
scoreboard players set @s ra_item_anim 2
```

Clip **order** in the animation JSON matters; clip **names** do not (same rule as [Raid zombie — Order matters, names do not](raid-zombie.md#order-matters-names-do-not)). The stem here is the **first-person** stem only.

## Animation speed (`ra_item_anim_spd`)

Playback speed is controlled separately from clip selection. Create a second **dummy** objective:

```text
ra_item_anim_spd
```

Example:

```mcfunction
scoreboard objectives add ra_item_anim_spd dummy
scoreboard players set @s ra_item_anim 2
scoreboard players set @s ra_item_anim_spd 150
```

The score is read on the **player** (same holder as `ra_item_anim`) on the **server** and synced to the client. Values are **hundredths** of a speed multiplier (same idea as [Player air dash](air-dash.md) `ra_dash_str`).

| Score | Behavior |
| ----- | -------- |
| **Missing objective** | Normal speed (`1.0×`, same as score `100`). |
| **No score set** | Same as `100` (player never assigned a value on this objective). |
| **0** | Freeze the current clip frame (`0.0×`) while `ra_item_anim` stays non-zero. |
| **50** | Half speed (`0.5×`). |
| **100** | Normal speed (`1.0×`). |
| **200** | Double speed (`2.0×`). |
| **1000** | Maximum (`10.0×`); higher scores are clamped. |

`ra_item_anim` picks **which** clip loops; `ra_item_anim_spd` scales **how fast** that clip advances. Changing speed mid-clip continues from the **current frame** (no jump); score `0` freezes that frame. Both only affect **first-person** main-hand playback for the **local player**.

## Client mod and visibility

| Topic | Notes |
| ----- | ----- |
| **Client mod** | **Required** on players who should see first-person animations (and TP models). |
| **Server mod** | Required so the item exists and scoreboards run; animation playback is client-side only. |
| **Other players** | Do **not** see your first-person animation. They see your **third-person** model (static). |
| **Main hand** | Animation applies only when the item is in the **main hand** (not offhand). |
| **Resource reload** | Press **F3+T** after editing `animations/item/` JSON. |
