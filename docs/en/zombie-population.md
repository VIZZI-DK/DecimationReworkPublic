# Zombie Population by Area

**English** · [Русский](../zombie-population.md)

## Getting started

1. Select an area with the selection tool or with `/deci region pos1` and `/deci region pos2`. Include the height of every intended floor.
2. Run `/deciserver zombies` to open the editor. The command and editor require operator permissions.
3. Enter a name and profile ID, then select **Create from Selection**.
4. Select the area and open **Spawn points → Generate**. The generator scans a grid with a four-block step and checks all heights in the selection using loaded chunks.
5. Move through the area and repeat generation for newly loaded sections. Existing points are preserved and nearby duplicates are skipped. Cancelling discards only the current run's results.
6. **Add here** adds a point beneath you. You can disable or delete points in the list. World markers show the area boundaries and up to 256 nearest points for two minutes. Green points are enabled and red points are disabled.

Spawns activate near living players outside Creative. Creative administrators do not trigger spawning, but their position and line of sight count when a point is checked.

## Profiles and settings

Default IDs are `city`, `village`, `military`, `police`, `hospital`, `industrial`, and `forest`. The profiles tab lets you change settings and zombie type weights. Change the ID and save to create a new profile. Use the assignment button to apply the selected profile to the selected area.

The server stores areas and profiles together in `config/decimation/zombies/config.json`, relative to its working directory. Successful saves are atomic and keep a backup. Revision checks reject stale saves when administrators edit concurrently.

Starting values include:

- Up to 300 managed zombies globally and 50 locally within 96 blocks.
- A population of 35 for most profiles, 18 for villages, and 8 for forests.
- A spawn distance of 35–110 blocks from an active player.
- A recovery delay of 600–900 real seconds after a zombie dies.
- Replenishment of 1–3 zombies every 30–60 seconds, including while players remain nearby.
- Return of an unseen zombie to the reserve after 90 seconds away from players and outside active combat.

These are initial settings. Capacity for 100 players requires load testing. Limits are 1,024 areas, 64 profiles, 20,000 spawn points per area, and 200,000 points in total.

| Profile key | Default | Effect |
| --- | --- | --- |
| `population`, `activeLimit` | 35, 35 | Population size and active zombie cap. Both are 18 for villages and 8 for forests. |
| `minDistance`, `maxDistance` | 35, 110 | Allowed spawn distance from an active player in blocks. |
| `delayMinSeconds`, `delayMaxSeconds` | 600, 900 | Random recovery delay for a population slot after death. |
| `intervalMinSeconds`, `intervalMaxSeconds` | 30, 60 | Replenishment interval. |
| `batchMin`, `batchMax` | 1, 3 | Number of zombies in one replenishment batch. |
| `virtualizeSeconds` | 90 | Delay before returning an unseen zombie to the reserve. |
| `migrationSeconds` | 120 | Population migration interval between neighboring areas. |
| `weights` | 80 main type, 10 crawler, 5 dog, 4 fat zombie, 1 brute | Relative type weights in the profile. |

At the top level, `enabled` controls the Director, `globalLimit` and `localLimit` set shared caps, and `areas` and `profiles` store areas and profiles. Each area has a dimension, bounds, priority, enabled state, profile, points, and neighboring IDs. A point stores XYZ, `yaw`, and `enabled`.

## Spawn rules and tracking

Spawn checks cover loaded chunks, ground support, liquids, the selected zombie type's dimensions, other entities, safe zones, and nearby houses. Spawning waits if any nearby player has a direct line of sight to the point. The check uses line of sight regardless of camera direction, so completely open fields may have no viable points. In overlapping areas, higher priority wins and stable ID order breaks ties.

Returning a zombie to the reserve preserves its health, equipment, and NBT without counting a kill. The world data registry `decimation_zombie_population_<dimension>.dat` stores managed zombies separately from chunk saves. After restarting, entities return gradually through ordinary spawn checks. Expired timers do not create an unlimited spawn queue.

Disabling the Director stops new managed spawns, restores legacy natural spawning, and preserves existing managed zombies. Disabling an area stops its replenishment. Lowering a cap leaves visible zombies in place. **Clear** removes active zombies in the area and starts recovery timers. **Refill** removes them and makes the reserve available, subject to normal spawn checks and batch limits. Both actions require confirmation.

Legacy unmanaged zombies, manual spawns, and third-party mods are outside these caps.

## Noise

Gunfire still attracts active zombies. Gunfire and explosions can also move a limited amount of population reserve. Enter neighboring area IDs as a comma-separated list. The editor shows the selected area's ID.

Reserve moves from the listed neighbors toward the area where the sound occurred, within the sound radius and the same dimension. Each cooldown allows at most one available reserve slot to trade places with one destination slot waiting for recovery. Total population and recovery obligations are preserved. Visible active zombies stay where they are, and the configured population remains the cap.

## Diagnostics

The editor shows active zombies, reserve, slots waiting for recovery, the most recent spawn rejection, and Director runtime. The generator shows progress and skipped candidates and can be cancelled. It stops early at the point cap.

A point can have enough collision clearance and still be unreachable. Disable such points manually. Simulation and generation use loaded chunks, so load the relevant sections when preparing a large map. Update both clients and server for the editor and its network protocol.
