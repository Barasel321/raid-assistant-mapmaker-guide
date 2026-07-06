# Raid animated item

[← Back to guide index](README.md)

The **raid animated item** is a held item with GeckoLib 3D visuals. Its animation pose is driven by a **player** scoreboard objective. Animations play **only in first person** for the **local player** while the item is in the **main hand** — other players do not see them.

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

See [Raid zombie — Visual variant](raid-zombie.md#visual-variant-raidvariant) for stem rules.

## Asset paths

Folder layout for item GeckoLib assets (namespace `raid-assistant`):

```text
assets/raid-assistant/
  geo/item/<stem>.geo.json
  textures/item/<stem>.png
  animations/item/<stem>.animation.json
```

- Default stem: `raid_item` (bundled in the mod).
- The **same** stem must be used for all three files for one look.
- Invalid or missing `RaidVariant` values fall back to `raid_item`. If geo is missing for a custom stem, the client also falls back to `raid_item`.

## Scoreboard objective

Create a **dummy** objective with this **exact** name:

```text
ra_item_anim
```

Example:

```mcfunction
scoreboard objectives add ra_item_anim dummy
```

The score is read on the **player** (not an entity). If the objective does not exist, the item behaves as if the score were **0** (frozen pose).

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

Clip **order** in the animation JSON matters; clip **names** do not (same rule as [Raid zombie — Order matters, names do not](raid-zombie.md#order-matters-names-do-not)).

## Client mod and visibility

| Topic | Notes |
| ----- | ----- |
| **Client mod** | **Required** on players who should see first-person animations. |
| **Server mod** | Required so the item exists and scoreboards run; animation playback is client-side only. |
| **Other players** | Do **not** see your first-person animation. Third-person views of the item stay static. |
| **Main hand** | Animation applies only when the item is in the **main hand** (not offhand). |
| **Resource reload** | Press **F3+T** after editing `animations/item/` JSON. |
