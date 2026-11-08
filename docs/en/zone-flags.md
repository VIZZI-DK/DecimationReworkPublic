# Zones, Flags, and Actions

**English** · [Русский](../zone-flags.md)

Run `/deciserver zones` or `/zone gui`. The commands and editor require operator permissions. Mark two corners with the selection tool and choose **Create from Selection**. The selection includes the boundary blocks. The editor can change either corner, the dimension, height, name, and priority.

## Storage and migration

Zones use schema 2 in `config/decimation/zones.json`. On first start, legacy zones receive IDs, names, dimension **0**, flags from their old type, and loot tags. Before writing the new file, the server creates `zones.json.legacy-<timestamp>.bak` and writes a migration summary to `zones-migration.txt`. Later saves are atomic and keep a `.backup` file. If reading fails, the existing file is preserved.

Undo reverts the latest save in the current server session. Reloading the file clears that undo. Revision checks reject stale saves when multiple administrators edit concurrently. Confirm before switching to another zone with unsaved changes.

## Flags

An unset value inherits from overlapping zones or the global default. In the editor, toggle values cycle through inherit, 1, and 0. A blank numeric field means inherit. Higher priority wins. At equal priority, 0 wins for boolean flags and stable ID order resolves numeric flags. The **Rules** tab shows the effective value and its source **at the administrator's position**.

- `safe` marks a safe zone. Apply the safe-zone preset to enable its protection flags.
- `pvp`, `damage`, `infected_damage`, `weapons`, `traps`, `projectiles`, `explosion_damage`, and `explosion_blocks` allow the corresponding action or damage. Projectiles and traps retain their 10-block protection buffer.
- `radiation` controls damage per pulse. 0 disables radiation, 1 preserves the legacy damage, and 10 is the maximum. Pulse timing, gas mask protection, and the global radiation setting still apply.
- `thirst` and `hunger` at 0 maintain hydration and hunger levels. `infection`, `bleeding`, and `broken_legs` at 0 prevent new conditions. Existing conditions persist.
- `build`, `break`, `pickup`, and `drop` control block placement, block breaking, item pickup, and item drops.
- `containers` controls opening world storage. Players can still use personal inventory and crafting menus. `loot` controls searching managed loot blocks. Clan, house, and other access checks still apply.
- `houses` at 0 forbids house placement inside and near the zone. Set `house_margin` to 0–1000 or leave it unset to use the house configuration. A zero margin still forbids placement inside the zone.
- `zombie_spawn` allows infected to spawn. The zombie type profile on the main tab changes the mix of **new** zombies at Director spawn points. The area's budget, timers, and saved zombies keep their existing settings. Leave the field blank to use the area's profile.
- `rspawn` allows players to use `/rspawn` outside active combat.
- `scary` applies the legacy scary-zone effect.
- `border` marks the area inside the permitted boundary. Outside damage requires a boundary enabled for the dimension and the global world border setting.

Migrated safe zones retain the protection exception for players with an active combat timer. Preset restrictions are controlled by individual flags.

Tags such as `military`, `police`, and legacy types preserve loot category compatibility. Enter group multipliers as `groupID=1.5,anotherGroup=0.5`, with values from 0 to 10. They multiply existing table modifiers and the Loot Director adjustment. Where zones overlap, priority selects one zone multiplier.

Raids on player structures require `break`. Bullets also check `projectiles`, and fragmentation grenades check both `explosion_damage` and `explosion_blocks`. These flags are evaluated at the affected section.

## Actions

Events can trigger on entry, exit, or while inside. An action can run a command, trigger a random respawn, send a message, or play a sound by resource ID. The server replaces `{player}` with the player's name. A command may start with a slash.

For example, select **On Enter**, set the type to **Command**, enter `say Welcome, {player}`, and enable **As console**. Commands execute on the server thread. With the toggle off, commands use the player's ordinary permissions. Changes to a zone with a console action require operator permissions and confirmation in the GUI. The server logs changes and command execution under `DecimationZones`.

Actions support a delay, a cooldown of at least one second, cancellation on exit, and a combat restriction. Delays use real time. Disconnecting clears the player's queue. Configuration changes cancel pending actions from the old revision, and restarting clears delayed actions. A combat restriction skips the action while combat is active. **While Inside** repeats at the configured interval.

The server runs at most 32 actions per tick and four per player. Teleporting clears stale entry actions. Normal entry in Creative does not trigger random respawn actions. Teleporting from the editor skips entry actions by default. Enable the entry toggle to exercise them, including in Creative. The teleport searches for a safe spot near the zone center without creating portals or loading the entire area.

Enter allowed or blocked command **names** separated by commas, without slashes or arguments. A zone allowlist still requires ordinary command permissions. At equal priority, the blocklist wins.

## In-game verification

After updating the client and server, check GUI scale, selection and preview, overlapping zones, multiple dimensions, legacy migration, delayed console actions, teleport with entry actions on and off, loot and weapon restrictions, and radiation. Preview displays boundaries for 120 seconds and leaves world blocks intact.
