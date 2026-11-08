# Clan Administration

**English** · [Русский](../clan-administration.md)

Update the client and server together because the clan protocol has changed. The external read-only `ClanApi` is still available.

## Bases and capture

Open the administration section from the clan menu. You can use it without belonging to a clan. It provides access to bases, the clan list, owner selection, settings, and the journal. Base teleports have been removed.

Set a capture schedule such as `18:00-22:00` and a time zone such as `Europe/Kyiv` or `UTC`. Separate multiple ranges with commas. Ranges can cross midnight. An empty schedule disables capture. When a window closes, any ongoing capture is cancelled and property stays with the owner.

The winner receives the existing storage banks and vehicles with the base. The previous owner keeps access until the capture succeeds. Deleting a base first creates an archive in `config/decimation/clans/archive`.

Migration backs up the data in `data.before-v3-*.json`. It stops old captures and preserves the recorded owner. Items beyond 27 slots in legacy banks move to additional banks without decoding the items. These banks are withdrawal-only and marked with `*`.

## Permissions and settings

The clan leader sets minimum roles for actions and individual banks. Server administration and the clan role hierarchy apply separately. Transferring leadership requires its own confirmation.

Administrators can adjust prices, maximum levels, and bonuses for all four upgrades, along with income by base tier, capture rewards, application and invitation expiry, and journal limits. Lowering a limit preserves existing property.

### Menu and file settings

Settings are stored in `config/decimation/clans/config.json`. The table lists defaults for a new server. The admin menu saves these fields immediately.

| Key | Default | Purpose |
| --- | --- | --- |
| `defaultSlots`, `maxBasesPerClan` | 10, 2 | Member and base limits before upgrades. |
| `baseStorageBanks`, `maxVehiclesPerBase` | 2, 2 | Storage banks and vehicle limit per base before upgrades. |
| `capturePoints`, `captureDurationSeconds` | 60, 60 | Capture points and duration in seconds. |
| `captureRewardCoins` | 10 | Capture reward. |
| `incomeTiny`, `incomeSmall`, `incomeMedium`, `incomeBig`, `incomeLarge` | 1, 1, 2, 3, 3 | Income for each base tier. |
| `verificationCost` | 1000 | Clan verification cost. |
| `upgradeBaseLimitCost`, `upgradeMemberSlotsCost` | 100, 75 | Base prices for base limit and member slot upgrades. |
| `upgradeStorageBanksCost`, `upgradeVehicleLimitCost` | 125, 175 | Base prices for storage bank and vehicle limit upgrades. |
| `invitationHours`, `applicationHours` | 24, 168 | Invitation and application expiry in hours. |
| `journalMaxEntries`, `journalRetentionDays` | 50000, 30 | Maximum entries and journal retention. |

Each entry in `upgradeRules` has `maxLevel` and `bonus`. The defaults are 5 and 1 for `base_limit`, `storage_banks`, and `vehicle_limit`, and 10 and 5 for `member_slots`. The bonus applies per upgrade level.

The file's `enabled` setting controls capture processing and defaults to `true`. `captureWindowSeconds` is a legacy field. Set the schedule separately for each base.

## Journal and recovery

The journal is stored in `config/decimation/clans/journal`. Clan leaders see their own clan's operations and administrators see all operations. Details include the base or bank and an item change summary. Only administrators can access full before and after snapshots. The files contain more data than the on-screen preview.

Property transfers are recorded as `PREPARED` and become `COMMITTED` after the save is verified. An unfinished or ambiguous transfer is locked as `REVIEW` for manual inspection. Compare the journal with the current property before confirming the current state. Confirmation keeps the current item state.

Confirmation checks player saves and standard Anvil chunks. Custom world storage may fail verification. A transfer succeeds only after its write succeeds. Synchronous verification saves can increase latency on slow disks. Back up the world separately.

## In-game verification

For the first deployment, use a copy of the world and keep the original clan data. Check the layout in-game and verify operation on your server implementation.
