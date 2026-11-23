# Build Over Workers

## UCP package

Place buildings or walls where friendly workers are standing. The workers move to a nearby free tile when one is available. Building and wall placement can be configured separately.

- Module directory: `module/`
- Targets: Stronghold Crusader 1.41 and Stronghold Crusader Extreme 1.41
- Requires UCP framework 3.0.7 and frontend 1.0.2 or a compatible later version


A UCP3 module for Stronghold Crusader and Stronghold Crusader Extreme. You can place buildings
and walls where your own (and, optionally, your allies') workers are standing; soldiers and
enemy units still block. Anyone standing where a new building or wall goes up - yours or the
AI's - is moved to the nearest free tile next to it and walks on, instead of being removed or
left stuck inside.

## What it does

Lets you place buildings and walls where your workers are standing. Every part can be
switched off in the settings.

### Buildings can be placed over workers

Before, any person standing on a tile made it red, so you had to wait for peasants and
workers to walk away. Now your own workers (peasants, farmers, woodcutters, workshop
workers, traders, the lady, children and so on) no longer get in the way. Soldiers,
tunnelers, engineers and enemy units still block the spot, just like before.

### Walls can be placed over workers

The same for walls.

### Allies' workers count as friendly

Workers of players on your team can be built over too. Switch this off to allow only your
own workers.

### Repairing a wall leaves people on top of it

Troops and people standing on top of a wall or stairs stay where they are when you build
over it to repair it. Only people on the ground underneath a new wall are moved aside.

### Nobody disappears

Anyone standing where a new building or wall goes up is moved to the nearest free tile
next to it, on the same side of your walls when possible, and walks on to where they were
going.

The AI gets the same treatment, for its buildings and for its walls, crenellations and
stairs: before, the game simply removed people standing where the AI built, or left them
stuck inside the building or wall until they vanished.
## What is in here

| Folder | |
|---|---|
| `module/` | the module itself - this is the source of truth, edited in place |
| `bench/` | the emulator bench: the module's own Lua runs against the real exe image, its assembly is assembled with UCP's own fasm.dll and then executed |
| `tools/publish.py` | copies `module/` into the game's module folder under a new version |

## Working on it

1. Edit under `module/`.
2. Run the bench until it is green: `bench/test_bow.py` (the placement tests for buildings and
   walls, remembering built-over tiles, AI walls, and moving the units afterwards), on both
   executables.
3. `python tools/publish.py --bump` - raises the last version slot and copies the module to
   `ucp/modules/build-over-workers-<version>`, leaving the build before it installed and
   clearing anything older.
4. In the UCP GUI press F5, then apply. `ucp-config.yml` is never edited by hand: the apply
   button moves the pin.
5. Commit and tag: `git commit -am "<version> - <what changed>" && git tag v<version>`.

The bench needs Python with `lupa`, `capstone`, `pefile` and `keystone-engine`, and a 32-bit
PowerShell for fasm.dll; it reads the executables from the game folder named in `shc.py`.
