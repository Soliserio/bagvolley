# BagVolley

**Version 2.0.0 · Author: Soliserio · MIT License**

BagVolley frees main Inventory space by moving matching items into configurable storage bags.

It scans the items you actually have, matches them against your sorting lists, and queues only the transfers needed. Missing items are silently ignored.

**Itemizer is not required for BagVolley sorting.** You can keep Itemizer installed for its separate retrieval and item-management features.

## Features

- Inventory-first sorting without commands or delays for absent items.
- Editable item lists and destinations.
- Solo and all-character macros.
- Preview, status, and cancellation commands.
- Bag-access and capacity checks.
- Protection for equipped, bazaar, and other locked items.
- Transfer confirmation before continuing.
- Optional sorting-list clearing with backups and confirmation.

BagVolley does not drop, sell, or indiscriminately empty your inventory. Unlisted items stay where they are.

## Requirements

- Windower 4.
- The **Send** addon for all-character commands.

Avoid running other inventory movers or manually moving items while BagVolley is sorting.

## Installation

1. Copy the `addons/bagvolley` folder into your Windower `addons` folder.
2. Copy `SortBags.txt` and `SortBagsAll.txt` into your Windower `scripts` folder.
3. For a first installation, copy the six supplied sorting lists into your Windower `scripts` folder:

   - `store_satchel.txt`
   - `store_sack.txt`
   - `store_case.txt`
   - `store_mogsafe.txt`
   - `store_mogsafe2.txt`
   - `store_storage.txt`

4. Load the addon:

   ```text
   //lua load bagvolley
   ```

### Updating an existing installation

Back up your addon and sorting lists first.

Replace the addon files and launchers as needed, but **keep your customized sorting lists** unless you deliberately want to restore the supplied defaults.

With sorting stopped, reload:

```text
//lua reload bagvolley
```

For all connected characters:

```text
//send @all lua reload bagvolley
```

Loading or reloading does not start transfers.

## Quick Start

Stand idle and close all menus.

### 1. Preview

```text
//bagvolley preview
```

Preview reports queued transfers, blocked items, and protected stacks. It does not move anything.

### 2. Start

```text
//bagvolley start
```

Start builds a fresh plan from your current inventory and sorting lists. If either changed after preview, preview again first.

### 3. Check status

```text
//bagvolley status
```

### 4. Cancel

```text
//bagvolley cancel
```

Cancellation stops future transfers. A transfer already dispatched may still finish.

## In-Game Macros

### Solo

Put this line in a macro:

```text
/console exec SortBags.txt
```

### All connected characters

Put this line in a macro and run it once from one character:

```text
/console exec SortBagsAll.txt
```

The group launcher uses Send and targets all connected clients. It is not limited to a fixed roster of eight.

Each character checks its own inventory, status, bag access, and capacity.

## Direct Group Commands

### Preview everyone

```text
//send @all bagvolley preview
```

### Start everyone

```text
//send @all bagvolley start
```

### Cancel everyone

```text
//send @all bagvolley cancel
```

## Customize Your Sorting Lists

The supplied lists preserve Soliserio’s setup as a starting point. Edit them to choose which items belong in each bag.

Stop sorting on all clients before editing shared lists.

All six files must exist, but empty files are allowed. Their filenames do not restrict the destinations you can use inside them.

### Move from Inventory into a bag

```text
put "Eye Drops" case all
```

### Retrieve from a bag into Inventory

```text
get "Eye Drops" case 1
```

### Move between bags

```text
move "Eye Drops" case sack all
```

### Move back into Inventory

```text
move "Eye Drops" case inventory 1
```

### Add a delay

```text
wait 1
```

### Add a comment

```text
-- Medicines stored in Mog Case
```

### Supported bag names

| In-game bag | Rule name |
|---|---|
| Inventory | `inventory` |
| Mog Safe | `safe` |
| Mog Safe 2 | `safe2` |
| Storage | `storage` |
| Mog Satchel | `satchel` |
| Mog Sack | `sack` |
| Mog Case | `case` |

### Rule behavior

- Quote item names.
- Use `all` or a positive item count.
- Prefer exact item names; `*` and `?` wildcards are supported.
- The first matching rule wins for each stack.
- Conflicting later rules do not move the same stack again during that run.
- New arrivals are handled on the next run.
- Normal `put` lists scan main Inventory.
- Explicit `get` and `move` rules can scan other source bags.
- Waits accept whole seconds from 1 through 60 and apply after actual transfers, not absent items.
- Temporary-bag moves, same-source-and-destination moves, semicolons, and arbitrary script commands are rejected.

Characters using the same Windower installation share its list files. Separate installations use their own copies.

## Clear Sorting Lists Only

This feature is for starting with blank routing lists.

**It does not move, drop, sell, or delete any game items.**

1. Stop sorting on every character.
2. Run this on **one character only**:

   ```text
   //bagvolley clear
   ```

3. Confirm within 30 seconds:

   ```text
   //bagvolley clear confirm
   ```

BagVolley backs up all six lists before emptying them.

Do not broadcast this command: characters on the same installation share the files. Write permission is required.

To restore a list, stop sorting everywhere and copy its saved backup over the matching list file.

## Safety Checks

BagVolley checks bag access and capacity before planning and again before sending a transfer.

- Inaccessible or full destinations are skipped.
- Mog Safe, Mog Safe 2, and Storage are checked for actual availability.
- Equipped, bazaar, and other locked stacks are protected.
- Item identity, slot, quantity, and augment data are rechecked before moving.
- Only one transfer is outstanding at a time.
- Matching source and destination count changes must confirm the transfer.
- A timeout stops the run without automatically retrying an uncertain move.
- Resting, combat, open menus, zoning, and logout block or stop sorting.

Full destinations are conservatively skipped even if partial-stack space might exist.

Transfer confirmation uses bag counts, not server transaction IDs. Avoid concurrent inventory changes from other addons or manual actions.

## Completion Messages

The completion summary reports:

- Confirmed transfers.
- Individual item quantities confirmed.
- Skipped transfers.
- Protected stacks.

A transfer and an item are not the same count. For example, three transferred stacks may contain 93 individual items.

## Bag Display Order

BagVolley moves items between bags. It does not rearrange the order displayed inside the game’s bag menus.

Use the game’s **Sort → Auto** manually when desired.

The experimental `organize` command was removed because it did not change the displayed order.

## Testing

### Confirmed in live testing

- Single-item transfer into Mog Case.
- Three-stack transfer containing 93 items.
- Storage transfer safely skipped outside the Mog House.
- Successful Storage transfer after returning inside.
- Successful transfers on all eight test characters.
- Resting characters blocked from starting.

One character initially reported a startup block after standing up. A later status check showed valid conditions, and the retry succeeded. The original cause was not established.

### Tested offline

- Rule parsing and inventory-first planning.
- Quantities, wildcards, and conflicting rules.
- Equipped-item protection.
- Bag-access and capacity checks.
- Stale slot, item, augment, and count detection.
- Transfer failure and timeout handling.
- Confirmation behavior.
- Sorting-list clearing and backups using disposable test files.

Not every item, bag type, timing condition, or configuration has been tested. Start with a preview and a small transfer test on your setup.

## Changes in Version 2.0.0

- Added an independent transfer engine.
- Removed the Itemizer dependency for sorting.
- Added inventory-first planning to avoid absent-item command spam and delays.
- Preserved editable lists and solo/group launchers.
- Included preview, status, cancellation, and backed-up list clearing.
- Added clearer idle and menu-state diagnostics.

## Author and License

**Author:** Soliserio  
**License:** MIT

See the included `LICENSE` file for the complete terms.

Itemizer is a separate addon and is not bundled with BagVolley. No Itemizer source is included in this release.

Public API reference: [Windower FFXI Functions](https://github.com/Windower/Lua/wiki/FFXI-Functions)

This project does not imply endorsement by Windower or the Itemizer authors.
