# Troubleshooting

[← Back to guide index](README.md)

| Symptom                                         | Things to check                                                                                                                                   |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Purple/black missing model                      | Pack not enabled, wrong path, wrong namespace, or typo in stem.                                                                                   |
| Default zombie look after setting `RaidVariant` | Stem invalid (uppercase, spaces, etc.), or **missing geo** for that stem (client falls back to `raid_zombie`). See [Raid zombie](raid-zombie.md). |
| Score does nothing                              | Objective must be exactly `ra_rz_anim`. Entity must be raid zombie type.                                                                          |
| Wrong clip after editing JSON                   | Resource reload **F3+T**; confirm **order** of `animations` entries, not names. See [Raid zombie — Order matters](raid-zombie.md#order-matters-names-do-not). |
| Dedicated server vs single-player               | Scores and summon run on the **server**; resource packs are per **client** for visuals. Custom models need the pack and mod on the client. Air dash only needs the mod on the **server**. Key binds need the mod on the **client**. |
| Hitbox tags ignored                             | Use `RaidHW` and `RaidHH`, not older names. Both accept float or double NBT. See [Raid zombie — Hitbox](raid-zombie.md#hitbox-width-and-height-raidhw-raidhh). |
| Mob still attacks after “disabling AI”          | Vanilla `NoAI:1b` stops all goals. For passive wander-only mobs, use `RaidAI:"wanderer"` instead. See [AI preset](raid-zombie.md#ai-preset-raidai). |
| Dash does nothing                               | `allowRaidDash` must be `true` on the server. Player must press sneak **after** leaving the ground (not hold from before jump). Spectators cannot dash. See [Air dash](air-dash.md). |
| Ability keys do nothing                         | Player needs the mod on the **client**. Binds are under **Raid Bindings** in Controls (defaults **R** / **V**). Check stat `raid-assistant:ability1` or `ability2`, or your scoreboard copy of it. See [Key binds](key-binds.md). |
| Clicks on item not detected                     | Item must be in main hand and tagged `#raid-assistant:click_detection` (datapack tag). For “used” counts, avoid holding the use key— it increments every tick while held. |
| Item animation does nothing                     | Player needs the mod on the **client**. Item must be in **main hand**. Objective must be exactly `ra_item_anim` on the **player** (sidebar display not required). Press **F3+T** after pack edits. See [Raid animated item](raid-animated-item.md). |
| Item animation speed ignored                  | Speed uses objective `ra_item_anim_spd` on the **player** (not the item or an entity). Player needs the mod on the **client**. Missing objective or unset player score = normal speed (`100`); use explicit `0` to freeze. See [Animation speed](raid-animated-item.md#animation-speed-ra_item_anim_spd). |
| Item shows default look after `RaidVariant`     | Stem invalid, or **missing geo** for that stem (client falls back to `raid_item`). |
| Third-person shows default TP look              | Expected if `geo/item/<stem>_tp.geo.json` (or your `RaidVariantTP` stem) is missing — client falls back to `raid_item_tp`, not the first-person stem. See [First-person vs third-person vs GUI stems](raid-animated-item.md#first-person-vs-third-person-vs-gui-stems). |
| Hotbar still shows first-person model           | `RaidVariantGUI` unset — defaults to the first-person stem. Set a separate GUI stem or `RaidVariantGUI:"none"` to hide. See [First-person vs third-person vs GUI stems](raid-animated-item.md#first-person-vs-third-person-vs-gui-stems). |
| Hidden item still visible in hotbar             | `RaidVariantGUI` must be exactly `none` (lowercase). Hiding applies only to inventory/hotbar/ground/frames, not held first- or third-person views. |
| GUI stem shows first-person model instead       | **Missing geo** for the `RaidVariantGUI` stem — client falls back to the effective first-person stem. Ship `geo/item/<guiStem>.geo.json` (+ texture). |
| Other player sees my item animation             | They should not — animations are local first-person only. Other players see the **static** third-person model (`_tp` stem). |
| `RaidPose` has no effect                        | Player needs the mod on the **client**. Invalid values fall back to `item`. Pose affects player arms only, not first-person geo. See [Hold pose](raid-animated-item.md#hold-pose-raidpose). |

For deep GeckoLib or Blockbench export errors, use GeckoLib’s documentation and logs; this guide only covers how Raid Assistant wires packs and commands.
