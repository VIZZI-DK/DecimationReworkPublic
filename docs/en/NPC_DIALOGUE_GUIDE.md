# NPC Dialogue, Conditions, and Conversation Objectives

**English** · [Русский](../NPC_DIALOGUE_GUIDE.md)

With operator permissions, aim at the NPC and open its settings from the administrator hub or run `/deciserver npc`. Apply dialogue changes with **Save** in the main editor.

## Response conditions

Select a dialogue node in the responses tab and open **Response Conditions**. All configured conditions must pass. You can invert each condition individually.

- Quest status can be unstarted, active, or completed. A quest ready for turn-in counts as active until turned in.
- Item checks match the exact item, variant, and NBT, and count items in the main inventory and equipped backpack. Checks leave items in place. Equipped armor and external storage are excluded.
- Humanity uses an inclusive range from 0 to 100.
- Memory checks whether this player has the specified choice key for this NPC.

Hide unavailable responses or leave them disabled with an explanation on hover. The server checks conditions again when a response is selected. Use **Refresh** to reevaluate conditions.

## Memory

The saved choice key records a response after the server accepts it. For example, use `helped_medic`. A key can contain up to 48 lowercase Latin letters, digits, underscores, and hyphens. Reference the same key in another node's memory condition. Opening the dialogue records the encounter. A choice key is recorded after selecting a response.

Memory belongs to each player–NPC pair and survives death, reconnects, and restarts. It stores up to 128 keys per NPC and up to 2,048 NPCs per player. Different NPCs using the same dialogue have separate memories.

Use **Greetings** to select nodes for repeat encounters and completed quests. For the latter, also select the quest. Completed-quest greetings take priority, followed by repeat encounters and then the standard opening line. Opening the administrative editor leaves encounter memory unchanged.

## Conversation objective

In the quest editor, select **Talk to an NPC**, then choose the assigned NPC and the required response. The objective counts once, after the player accepts the quest and the server verifies the selected response. Progress requires the chosen response from the specific NPC. Players turn the quest in to the normal quest NPC.

Save the dialogue with the required response and bind it to the NPC before configuring the objective. Changing response text or order preserves its ID. Deleting a response or reassigning the dialogue can make the objective unavailable. Players then cannot accept the quest, and those who already accepted it see the reason.

## Preview

Open the preview state controls and set quest statuses, item counts, humanity, memory, and encounter type. The preview uses the same condition logic as the server and keeps live player data intact. Selected responses can write choice keys to the local simulation.

The node tab searches by ID and text. The unreachable-node warning checks paths from all greetings. Use the preview to check whether each branch's conditions can be met.
