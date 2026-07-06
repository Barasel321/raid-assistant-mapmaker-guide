# Player air dash

[← Back to guide index](README.md)

Raid Assistant can give players a **horizontal air dash** for parkour or combat maps. This is independent of raid zombies; enable it with gamerules on the **server**.

|                 |                                                                                                                                                                                                                                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Gamerules**   | Under **Raid Assistant** on the *Game Rules* screen: `allowRaidDash` (on/off). Per-player cooldown and strength use **scoreboard** objectives (see below).                                                                                             |
| **Default**     | `allowRaidDash` is `false`. If scoreboard objectives are not set up, cooldown defaults to **20** ticks (~1 second at 20 TPS) and strength to **75** (0.75 blocks/tick).                                                                               |
| **Turn on**     | `/gamerule allowRaidDash true`                                                                                                                                                                                                                       |
| **Turn off**    | `/gamerule allowRaidDash false`                                                                                                                                                                                                                      |
| **Cooldown**    | Scoreboard objective `ra_dash_cd` (dummy), holder = **player**. Ticks between dashes. **0** = no cooldown. Example: `scoreboard objectives add ra_dash_cd dummy` then `scoreboard players set @s ra_dash_cd 60` for 3 seconds at 20 TPS.           |
| **Strength**    | Scoreboard objective `ra_dash_str` (dummy), holder = **player**. Horizontal speed in **hundredths** — `75` = 0.75 blocks/tick (built-in default). Example: `scoreboard objectives add ra_dash_str dummy` then `scoreboard players set @s ra_dash_str 100` for a stronger dash. |
| **How to dash** | Jump (or leave the ground). While **airborne**, **press sneak** once. You move horizontally in the direction you are **looking** (flat to the ground). Holding sneak from before you jump does not count; you need a **new** sneak press in the air. |
| **Spectators**  | Cannot dash.                                                                                                                                                                                                                                         |

**Map setup:** Enable the gamerule in your lobby (command block, `level.dat` via editor, or datapack `gamerule` on load), or leave it off and turn it on only in dash-enabled areas via command blocks. Dash logic runs server-side; **vanilla clients** on a server with this mod still receive dash motion. The client mod is **not** required for dash (only the server needs Raid Assistant for that feature).

See also: [Quick reference — Player air dash](quick-reference.md#player-air-dash) and [Troubleshooting](troubleshooting.md).
