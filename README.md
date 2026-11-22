# Infection - server files

Server files and setup notes for **Infection**, a DayZ mod in which a player
brought down by the infected does not die: they turn, and play on as one of
the infected, with their own abilities and a horde to command.

- **Players** need only the mod itself: subscribe to *Infection* on the Steam
  Workshop.
- **Servers** need the mod and the server-side part in this repository.

**Download the latest server files:**
https://github.com/karmaroo/infection-server/releases/latest/download/Infection-ServerFiles.zip

Questions, bug reports and news: [Discord](https://discord.gg/BkH42xwuP)

## What is here

| Path | What it is |
|---|---|
| `@InfectionServer/` | The server-side part of the mod. Never sent to players. |
| `keys/` | The public key the mod is signed with, for servers that verify signatures. |
| `LICENSE.txt` | Terms of use. Repacking or re-uploading the mod is not allowed. |

Each release has the same files as one zip.

## Installing on a server

1. Copy the `@Infection` folder (from the Steam Workshop) into your server
   folder.
2. Copy `@InfectionServer` from here into your server folder as well, next
   to `@Infection`.
3. If your server verifies signatures, copy the `.bikey` file from `keys`
   into your server's `keys` folder.
4. Add both to the server's start parameters:

   ```
   -mod=@Infection -servermod=@InfectionServer
   ```

   Use `-servermod`, not `-mod`, for `@InfectionServer`.
5. Start the server once and let a player join. It writes
   `Infection\Settings.json` into its profile folder (the folder given by
   `-profiles=`). Stop the server, edit the settings you want, and start it
   again.

## Updating

When the mod updates, replace both folders. An `@InfectionServer` from one
version will not necessarily work with an `@Infection` from another. The
release number here matches the version of the mod on the Workshop.

## Settings

`Settings.json` lists every setting with its default. It is rewritten each
time the server starts, so settings added by a new version appear in it
automatically. If the file cannot be read (a typing mistake, say), it is
left untouched and the defaults are used; the server log says so.

| Group | Settings |
|---|---|
| Rising | `RiseDelaySeconds`, `InfectedHitWindowSeconds`, `TransferGear`, `RemoveOldBody`, `GaitWeightA` to `GaitWeightD` (how often each infected gait is given) |
| Who rises | `RiseChance` (0-1, for deaths by the infected), `RiseChanceOtherDeaths` (0-1, for every other death; 0 by default, 1 with `RiseChance` at 1 makes every death turn), `MaxRisen` (most risen players kept at once; 0 = no limit), `RiseWhitelist` (Steam64 IDs of the only players who may rise; empty = everyone) |
| The cure | `CureEnabled`, `CureItem`, `CureSeconds`, `CureRequiresRestraint`, `CureUnconsciousSeconds`, `CureHealth` (0-1) |
| Power | `PowerOnRise` (0-1), `PowerDrainMinutes`, `PowerPerHit`, `PowerKillHuman`, `PowerKillAnimal`, `EmpoweredPower` (0-100) |
| Feasting | `FeastBiteSeconds`, `FeastHealth`, `FeastBlood`, bites per meal (`FeastBitesPer...`), power per bite (`FeastPower...`) |
| Abilities | `Cooldown...Ms`, `Damage...` (a CfgAmmo class per attack), `ClawBloodLoss`, `MarkRange`, `ScentSeconds`, `ScentRangeMin`, `ScentRangeMax` |
| Horde | `HordeMax`, `HordeMin`, `RallyRadius`, `HordeAttackRange`, `HordeChargeDistance`, `HordePatrolRadius`, `HordeStragglerSeconds`, `FeralSeconds`, `HordeFeastMax` |
| Toughness | `RisenBodyDamage` (0-1): how much of a hit anywhere but the head a risen player takes. 1 as a survivor, 0 only head hits hurt them |
| Doors | `RisenCanBreakDoors`, `HordeCanBreakDoors`, `DoorHitSeconds`, `DoorHits` |
| Voice | `RisenVoice`: what others hear when a risen player talks. 0 normal, 1 mumbling (default), 2 silent |
| Diagnostics | `DebugLog` |

Risen players are recorded in `Infection\Risen.json` in the same folder.
Removing a player's entry returns them to a normal survivor.

## The cure item

Survivors cure a risen player with the Z-Cure injector, class
`Infection_CureInjector`. The game does not spawn it by itself: until you
add it to your loot there is no cure on your server. Add this to
`db\types.xml` in your mission folder (rare, in medical buildings; change
the numbers to taste), or sell it at a trader:

```xml
<type name="Infection_CureInjector">
    <nominal>4</nominal>
    <lifetime>14400</lifetime>
    <restock>3600</restock>
    <min>2</min>
    <quantmin>-1</quantmin>
    <quantmax>-1</quantmax>
    <cost>100</cost>
    <flags count_in_cargo="0" count_in_hoarder="0" count_in_map="1" count_in_player="0" crafted="0" deloot="0"/>
    <category name="tools"/>
    <usage name="Medic"/>
</type>
```

To use the game's PO-X antidote as the cure instead, with no loot changes,
set `CureItem` to `AntiChemInjector` in `Settings.json`.

## Terms

Play the mod and run it on your servers by subscribing on the Steam
Workshop. Do not repack it, re-upload it or modified versions of it, include
its files in another mod or server pack, or sell it or access to it. The
full terms are in `LICENSE.txt`.
