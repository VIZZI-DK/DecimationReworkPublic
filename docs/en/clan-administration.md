# Clan Administration

**English** · [Русский](../clan-administration.md)

Open the clan menu and choose **Administration**. Operator permissions are required. Under **All clans**, search by name or tag, open a clan's details, or change its verification status. The same menu also opens settings and the journal.

## Treasury and permissions

Members can deposit caps into the clan treasury. Withdrawals require the configured role, which defaults to officer or higher. The treasury funds verification and member slot upgrades. The clan leader can set minimum roles for managing members, managing the blacklist, and withdrawing funds. Transferring leadership requires confirmation.

## Server settings

Settings are stored in `config/decimation/clans/config.json`. The menu saves changes immediately. Restart the server after editing the file manually.

| Key | Default | Purpose |
| --- | --- | --- |
| `defaultSlots` | 10 | Initial member limit for new clans. |
| `verificationCost` | 1000 | Verification price in caps. |
| `upgradeMemberSlotsCost` | 75 | Base price of a member slot upgrade. |
| `upgradeRules.member_slots.maxLevel`, `upgradeRules.member_slots.bonus` | 10, 5 | Maximum upgrade level and additional member slots per level. |
| `invitationHours`, `applicationHours` | 24, 168 | Invitation and application lifetimes in hours. |
| `journalMaxEntries`, `journalRetentionDays` | 50000, 30 | Journal entry limit and retention in days. |

## Journal and recovery

The journal is stored in `config/decimation/clans/journal/`. Clan leaders see their own clan's operations. Operators see all entries and detailed transfer evidence. A transfer starts as `PREPARED` and becomes `COMMITTED` after its save is verified. An unfinished transfer is marked `REVIEW`. Compare the saved balances with the journal before confirming. Confirmation keeps the current balances and lifts the lock.

## Upgrading from an older version

When migrating an older snapshot, the server saves the original as `config/decimation/clans/data.before-v4-*.json`. Former bases, storage, and garage contents remain in that copy. The new snapshot retains clans, members, treasury balances, and member slot upgrade levels. Separate base and storage files from older formats stay on disk.
