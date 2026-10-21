BagVolley
Version 2.0.0 | Author Soliserio | MIT License
Purpose
Clear space in main Inventory by moving only items actually present that match your editable routing lists. Unlisted items stay put. This is not a drop/sell tool and does not indiscriminately empty your inventory.
Requirements and coexistence
Requires Windower 4. Group commands additionally require Send. Itemizer is NOT required for BagVolley sorting. You may keep Itemizer for its item retrieval, ninja tools, stacking and one-off transfer commands. BagVolley does not disable Itemizer or claim its commands. Avoid simultaneous inventory movement by Itemizer, GearSwap, other addons or manual actions while a BagVolley run is active, since concurrent changes affect confirmation.
Installation and updates
Copy addons/bagvolley into Windower/addons/. Copy scripts/SortBags.txt and scripts/SortBagsAll.txt into Windower/scripts/. For a FIRST installation, copy the six scripts/store_*.txt default lists too. For an UPDATE, keep your customized store_*.txt files; do not overwrite them unless you deliberately want to replace your routes with these defaults. Back up your existing addon and lists before installation. With sorting stopped, load/reload locally: //lua reload bagvolley Group reload, once from one character: //send @all lua reload bagvolley Installation alone does not move items or clear lists.
Quick start commands
//bagvolley preview
//bagvolley start
//bagvolley status
//bagvolley cancel
Preview sends no moves. Start runs the CURRENT lists and CURRENT inventory snapshot, so preview again if you change items or rules before starting. Remain standing idle with menus closed. Resting, combat, menus, zoning and logout stop or block sorting. Cancellation stops future moves; a dispatched move may still finish. A pending/uncertain move imposes a restart cooldown.
Macros and group commands
Solo: /console exec SortBags.txt
Group: /console exec SortBagsAll.txt
Run the group macro ONCE from one character. Send addresses all connected clients, not a hard-coded roster of eight.
Group preview: //send @all bagvolley preview
Group start: //send @all bagvolley start
Group cancel: //send @all bagvolley cancel
Customize sorting lists
The supplied defaults preserve Soliserio's existing setup. Edit these plain text files while all clients are finished sorting: store_satchel.txt, store_sack.txt, store_case.txt, store_mogsafe.txt, store_mogsafe2.txt, store_storage.txt. All six files must exist; empty files are allowed. Their order is the order above. Their filenames do not restrict the destination of individual rules. Examples:
put "Eye Drops" case all
get "Eye Drops" case 1
move "Eye Drops" case sack all
move "Eye Drops" case inventory 1
wait 1
-- A comment
put moves Inventory -> named bag. get moves named bag -> Inventory. move requires explicit source AND destination. Use all or a positive quantity. Bag API names include inventory, safe, safe2, storage, satchel, sack, case. The temporary bag and same-source/destination moves are rejected. Quoted names required; wildcard * and ? accepted. Prefer exact names. Semicolons and arbitrary script commands are rejected before any moves. Waits are whole seconds from 1 through 60. A wait following a rule supplies its post-transfer delay, at least one second, only after an actual move. Absent items do NOT cause errors or per-rule waiting. First matching rule wins for each source stack. Conflicting later rules do not bounce that stack elsewhere. New arrivals wait for the next sorting run. Normal put lists scan main Inventory. Explicit get/move rules scan their specified source bags too. Each client uses its own installation's lists.
Clear sorting lists only
With sorting stopped on ALL clients, run on ONE client only:
//bagvolley clear
//bagvolley clear confirm
Confirm within 30 seconds. This backs up all six list files beside their originals with unique before_clear names, then empties the LIST FILES ONLY. No items move, drop, sell or disappear. Do not broadcast this command: characters using the same installation share these files. Running snapshots on other clients are not cancelled by changing files. Write permission is required. To restore, stop sorting everywhere and copy each desired backup over its matching original. This command was NOT run on the supplied lists.
Safety and results
Checks actual bag access, including Mog Safe, Mog Safe 2 and Storage, before planning and again before dispatch. Inaccessible or full destinations are skipped; missing items are quiet. Full sources may still be emptied. Equipped, bazaar and other nonzero-status items are protected. Slot item ID, count and extdata are rechecked immediately before requesting a move. Only one transfer is outstanding at a time. Matching source and destination count changes must confirm it; timeout stops without automatic retry. Completion reports confirmed transfers and individual item quantities, skipped transfers, and protected stacks. It is not a promise every item fit. Full destinations are conservatively skipped even if partial-stack capacity may exist. Confirmation is based on bag counts, not server transaction IDs.
Display order
Use the GAME'S Sort -> Auto manually after transfers if desired. Native stack cleanup did not reorder the displayed Mog Case list in testing, so the experimental organize/tidycase command was removed. No alphabetical sorting or automatic menu control is provided.
Testing
User-confirmed live: single Eye Drops transfer, three-stack/93-item transfer, Storage blocked outside and successful inside Mog House, and successful transfers on all eight characters. Resting characters were correctly blocked. Kogawolf's earlier block remains unexplained; status subsequently showed idle, menus closed and logged in, and his retry succeeded with safeguards intact. Offline Lua 5.1 tests: parser, inventory-first queue, quantities, wildcards, conflicting rules, equipped protection, bag-access/capacity checks, stale slot/augment changes, transfer failure/timeout, and confirmation behavior. List clearing/backups tested with disposable files, not the user's live lists. Not every item, bag type, timing condition or configuration has been tested.
Changes in version 2 0 0
Independent transfer engine; no Itemizer dependency for sorting. Inventory-first planning removes absent-item command spam and delays. Retained existing editable lists, solo/group macros, preview, status, cancel, and optional backed-up list clearing. Added precise idle/menu diagnostics.
Attribution and license
BagVolley code author: Soliserio. Itemizer is a separate addon and is not bundled. No Itemizer source is included in this release. Public API references: https://github.com/Windower/Lua/wiki/FFXI-Functions Licensed under the MIT License. Copyright (c) 2026 Soliserio. See the included LICENSE file for permissions, conditions and disclaimers. Nothing in this package implies endorsement by Windower or Itemizer authors.
