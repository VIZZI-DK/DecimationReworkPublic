# Decimation Rework Server Settings

**English** · [Русский](../SERVER_SETTINGS_RU.md)

Paths are relative to the server's working directory. The mod's main settings file is `config/decimation/server.properties`. The `server.properties` at the root belongs to Minecraft. Run `/deciserver reload` after changing the mod's main settings. Use the subsystem-specific instructions below for other files.

## Main settings file

Defaults are for a new server. Boolean settings accept `true` or `false`. The server clamps numeric settings with ranges to the documented bounds.

### Game mode, players, and combat

| Key | Default | Effect |
| --- | --- | --- |
| `dmServer` | `false` | Enables deathmatch rules and kit NPCs. |
| `enableKillFeed` | `true` | Enables the kill feed. |
| `enableDeathScreen` | `true` | Enables the Decimation death screen. Reloading synchronizes this setting with connected clients. |
| `requireMapItem` | `true` | Requires regular players to carry a map item to open the full-screen map. |
| `allowRandomSpawns` | `false` | Picks a random point from the mod's spawn list when a player respawns. |
| `enableLegBreaking` | `true` | Allows leg fractures under the mod's rules. |
| `enableBleeding` | `true` | Allows bleeding. |
| `enableInfection` | `true` | Allows infection. |
| `enableIrradiation` | `true` | Allows radiation zones to irradiate players. Zone rules control intensity. |
| `disableThirst` | `false` | Set to `true` to disable thirst. |
| `disableExtraArmor` | `false` | Set to `true` to disable the mod's extra armor pieces. |
| `disableCorpses` | `false` | Set to `true` to stop creating corpses on death. |
| `corpseLifetimeMinutes` | `60` | Corpse lifetime, 1–525,600 minutes measured in world ticks. |
| `enableInventorySync` | `true` | Synchronizes inventory and equipment data through the mod. |
| `enableChatBubbles` | `true` | Shows a typing indicator above the player. |
| `disableClimbing` | `false` | Set to `true` to disable Decimation's climbing mechanic. |
| `enableSafezoneGrace` | `true` | Applies the safe-zone grace timer to damage checks between players. |

### World, construction, and loot

| Key | Default | Effect |
| --- | --- | --- |
| `enableSupplyDrops` | `true` | Enables automatic supply drops at configured points. |
| `enableBlockBreaking` | `false` | Globally allows breaking ordinary blocks in Survival. Fortifications, Creative, and zones have separate checks. |
| `blockRegenerationMinutes` | `1` | Delay before restoring blocks in the regeneration queue, 1–525,600 minutes. |
| `enableBlockPlacing` | `false` | Globally allows placing ordinary blocks in Survival. Fortifications, Creative, and zones have separate checks. |
| `enableItemPickup` | `false` | Allows automatic pickup of dropped items in Survival on a dedicated server. |
| `disableLootSystem` | `false` | Set to `true` to disable managed loot. |
| `lootRespawnTime` | `900` | Seconds for the legacy loot timer and the initial settings of new sources. Each source in `loot/sources.json` has its own `respawnMillis`. |
| `maxDemonCount` | `5` | Demon spawn limit threshold, 0–1,000,000. |
| `disableModdedMonsterSpawns` | `false` | Set to `true` to disable registration of the mod's ordinary monster spawns. Restart to apply. Managed zombies use the population editor. |
| `enableDynamicWeather` | `true` | Enables dynamic weather control. |
| `serverFogDensity` | `0.008` | Normal fog density, 0–1. |
| `serverFogRainDensity` | `0.1` | Rain fog density, 0–1. |
| `enableWorldBorder` | `false` | Enables damage outside the permitted area in dimensions with a zone border configured. |

### Connections, moderation, and external services

| Key | Default | Effect |
| --- | --- | --- |
| `allowAdditionalMods` | `false` | Set to `true` to allow additional client mods. Decimation versions must still match. |
| `allowedClientMods` | `OptiFine` | Comma-separated, case-insensitive list of allowed client mod IDs. Installed server mods also count toward the allowed list. |
| `enableAntiDupe` | `false` | Enables inventory checks for duplication. |
| `enableEnchanting` | `false` | With the duplication check enabled, `false` allows it to strip enchantments from items it finds. |
| `enableDebugMode` | `false` | Enables detailed mod debug output, including network message diagnostics. |
| `enableCommunityServerList` | `false` | Allows registering the server on the community list. |
| `enableOfficialStats` | `false` | Allows sending official statistics when a key is configured. |
| `officialServerApiKey` | Empty | Key for official statistics. Keep it in the private server configuration. |

The loader writes `serverType=ZOMBIE_SURVIVAL` when it creates the file. It reads `enableAutoUpdating`, `disableUpdates`, `motdImage`, `disableClanTags`, and `maxEntities`, which the current implementation leaves unused. The built-in update check reports that updating is disabled. The loader removes the legacy keys `communityServerId`, `communityServerKey`, `officialServerId`, `officialServerAuthCode`, `disableVPNConnections`, and `disableLootDownload`.

## Shared settings file

The mod reads `config/decimation/common.properties` at startup.

| Key | Default | Effect |
| --- | --- | --- |
| `allowVanillaMonsters` | `false` | Allows vanilla monsters. |
| `constructionHammerBlocks` | `deci:BlockMetalWall` | Comma-separated list of block IDs that the construction hammer supports. |

## Player houses

Edit `config/decimation/houses.properties` and restart the server to apply manual changes. Houses have their own ownership, storage, fuel, and garage system. Fences, watchtowers, and flagpoles use the player construction system.

| Key | Default | Range | Effect |
| --- | ---: | --- | --- |
| `zoneExclusionRadius` | 200 | 0–100,000 | Distance from a zone within which house placement is forbidden. The `house_margin` flag can override this distance. |
| `minimumHouseDistance` | 5 | 0–1,000 | Minimum distance between houses in blocks. |
| `maxHousesPerPlayer` | 1 | 1–1,000 | Maximum houses per player. |
| `storageSlots` | 54 | Fixed | Number of storage slots. The loader writes 54 back to the file. |
| `garageCapacity` | 3 | 0–64 | Maximum vehicles in the garage. |
| `garageRadius` | 24 | 4–128 | Garage range in blocks. |
| `fuelMinutesPerLog` | 180 | 1–525,600 | Minutes of fuel per log. |
| `armorDamageIntervalMinutes` | 30 | 1–525,600 | Interval between wear cycles for stored armor. |
| `weaponConditionLossPerCycle` | 100 | 1–10,000 | Weapon condition lost per wear cycle. |
| `storedVehiclePartDamageIntervalMinutes` | 360 | 1–525,600 | Interval between wear cycles for vehicle parts in the garage. |
| `maintenanceIntervalSeconds` | 30 | 5–3,600 | Upkeep processing interval. |

## Subsystem files and how to apply changes

Paths below are relative to `config/decimation/` unless marked as world data or relative to the server root.

| Subsystem | Files | How to edit and apply changes |
| --- | --- | --- |
| Zones | `zones.json` | The zone editor saves changes immediately. After a manual edit, use **Reload file** or `/zone reload` with operator permissions. [Schema and flags](zone-flags.md) |
| Legacy turfs | `turfs.json` | Use `/turf create`, `/turf remove`, and `/turf reload`. |
| Clans | `clans/config.json`, `clans/data.json`, `clans/journal/` | Use the admin menu for verification pricing, member slot upgrades, application and invitation expiry, and journal limits. Back up both the data and journal. [Clans](clan-administration.md) |
| Loot | `loot/groups/*.json`, `loot/tables/*.json`, `loot/sources.json`, `loot/state/` | The editor saves changes. After a manual edit, run `/deciserver loot reload`. Generated container contents are stored separately from generation rules. [Loot](loot-system-admin.md) |
| Loot Director | `loot-director/state.json` | Use the Director tab or `/deciserver loot director`. The file stores settings and accumulated state. |
| Zombie population | `zombies/config.json`, state in world data | The editor changes global limits, areas, points, profiles, and noise settings. Restart after manual edits. [Population settings](zombie-population.md) |
| Map | `map/map.json`, `map/regions/*.json`, `map/markers.json`, `map/icons.json` | `/deci map reload` reloads the map, regions, and markers. Edit the icon catalog in the GUI, or restart after manual edits. [Map](MAP_ADMIN_GUIDE.md) |
| NPCs and quests | `quests/catalog.json`, NPC bindings and progress in world data | Use the dialogue and quest editor. Restart after editing the catalog manually. [Dialogue](NPC_DIALOGUE_GUIDE.md) |
| Traders | `traders.json`, `trades/` | `traders.json` stores NPC placement. `trades/` holds catalogs and custom types. `/trader reload` reloads stock and respawns traders. |
| Player spawns and supply drops | `spawnpoints.properties`, `supplydrop.properties` | Use `/deciserver playerspawn` and `/deciserver supplydrop` to edit the lists. |
| Weapons | `weapon-tuning/weapons/`, `weapon-tuning/attachments/`, `weapon-tuning/manifest.json` | The editor saves the profile and synchronizes clients. Restart after manual edits. [Weapons](weapon-tuning-admin.md) |
| Player hitboxes | `player_hitboxes.json` | The editor saves changes on the server. Restart after manual edits. |
| Vehicles | `vehicledefs/vehicles/` | Use `/decivehicles reload` or `/decivehicles sync`. |
| City power | `decimation_city_power` in world data | Open `/deciserver power`. Districts and power are separate for each dimension. |
| Houses | `houses.properties`, `decimation_houses` in world data, `logs/houses/` from the server root | The settings file controls limits and upkeep. Use the menu to manage individual houses and pause all upkeep. |
| Flagpoles and player bases | `decimation_base_flags_<dimension>` in world data and structure NBT | The flag radius is 60 blocks. It can be refreshed once a day and stays raised for up to five real days. Its lifetime is 45 days from the last refresh. These values are built into the current version. |

## Distant terrain LOD

The server reads the `lod` section of `config/decimation/chunk-render.cfg` at startup. Cache and rendering settings apply to the client. Generated region data is stored in the world's `decimation_chunk_render` directory. See the [command reference](ADMIN_COMMANDS.md#distant-terrain-generation) for generation commands.

| Key | Default | Range | Effect |
| --- | --- | --- | --- |
| `enabled` | `true` | | Enables LOD. |
| `additionalDistanceChunks` | 2 | 0–server limit | Additional render distance in chunks. |
| `maxAdditionalDistanceChunks` | 32 | 1–32 | Upper bound for additional render distance. |
| `generationSamplesPerTick` | 1 | 1–64 | Samples processed per generation tick. |
| `maxGenerationRadiusChunks` | 512 | 1–512 | Maximum generation radius. Values of 0 or less become 512. |
| `generationTickBudgetMicros` | 4000 | 500–20,000 | Generation time budget in microseconds per tick. |
| `generationWorkerThreads` | 1–4 based on CPU count | 1–4 | Number of generation workers. |
| `generationQueueRegions` | Twice the worker count | 1–16 | Maximum queued regions. |
| `maxRegionPayloadMiB` | 16 | 1–16 | Maximum transfer size for one region. |
| `maxRegionTransfersPerPlayer` | 2 | 1–8 | Limit on region transfers to one player. |
| `maxVboMemoryMiB` | 256 | 1–2,048 | Memory budget for rendering buffers. |
| `maxDecodedRegions` | 128 | 1–1,024 | Maximum decoded regions held in memory. |
| `diskCacheMiB` | 1024 | 1–16,384 | Client disk cache budget. |
| `clientMeshThreads` | 1–2 based on CPU count | 1–2 | Client mesh-building workers. |
| `clientMeshQueueLayers` | 32 | 4–128 | Maximum layers in the client mesh queue. |

When moving a server, copy the configuration and world directory together. Administrative actions can change either one. Deleting a zone changes JSON, while deleting a house changes world data. Stop the server before replacing files that a subsystem may write in the background.
