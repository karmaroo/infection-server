# Infection - server files

Server files and setup notes for **Infection**, a DayZ mod in which a player
brought down by the infected does not die: they turn, and play on as one of
the infected, with their own abilities and a horde to command.

- **Players** need only the mod itself: subscribe to
  [Infection on the Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3839248989).
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

1. Copy the `@Infection` folder (from the
   [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3839248989))
   into your server folder.
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

How values are written in the file:

- On/off settings are `1` (on) or `0` (off).
- Chances and fractions run from `0` to `1`: `0.25` is 25%.
- The server writes some decimals back in a long form, such as
  `0.30000001192092898` for `0.3`. That is the same value; type `0.3`.
- Changes take effect the next time the server starts.

### Rising

| Setting | Default | What it does |
|---|---|---|
| `RiseDelaySeconds` | `15` | Seconds between going down and rising. At least 1. |
| `InfectedHitWindowSeconds` | `300` | A death with no killing blow (bleeding out, shock) still counts as a kill by the infected if one hit the player within this many seconds. |
| `TransferGear` | `1` | Move the player's clothing, and everything in it, onto the risen body. |
| `RemoveOldBody` \* | `1` | Remove the old corpse once the player has risen, so the risen body is the only one. Anything left on it drops to the ground. |
| `GaitWeightA` \* | `55` | How often the most upright infected gait is given to a newly risen player, relative to the other three. `0` removes a gait from the draw. |
| `GaitWeightB` \* | `20` | The same, for the second gait. |
| `GaitWeightC` \* | `20` | The same, for the third gait. |
| `GaitWeightD` \* | `5` | The same, for the deepest hunch. |

\* Best left at the default. These are there for fine tuning; most servers
have no reason to change them.

### Who rises

| Setting | Default | What it does |
|---|---|---|
| `RiseChance` | `1.0` | Chance (0-1) that a player killed by the infected rises. Those who do not rise die as in the unmodded game. |
| `RiseChanceOtherDeaths` | `0.0` | Chance (0-1) that a player who dies of anything else rises: gunshots, falls, hunger, other players. `0`: only the infected turn players. `1`, with `RiseChance` at `1`: every death turns. A player who takes their own life, or respawns from the menu, never rises from it. |
| `MaxRisen` | `0` | Most risen players the server keeps at once, counting those who are logged out. While it is reached, nobody else rises. `0` = no limit. |
| `RiseWhitelist` | `[]` | Steam64 IDs of the only players who may rise, for example `["76561198000000001", "76561198000000002"]`. Empty = everyone may. |

### The cure

| Setting | Default | What it does |
|---|---|---|
| `CureEnabled` | `1` | Let survivors turn a risen player back into a survivor. |
| `CureItem` | `"Infection_CureInjector"` | Class name of the item that cures; items inheriting from it count too. It is used up. The default is the mod's Z-Cure injector, which you have to add to your loot (see below). `"AntiChemInjector"` makes the game's PO-X antidote the cure; Z-Cure injectors then work as well. Empty switches the cure off. |
| `CureSeconds` | `10.0` | Seconds the survivor has to keep at it. At least 1. |
| `CureRequiresRestraint` | `1` | The risen player has to be restrained (handcuffs, rope and so on) first. |
| `CureUnconsciousSeconds` | `15.0` | Seconds the risen player lies unconscious after the cure, before waking as a survivor. |
| `CureHealth` | `0.5` | Health the cured survivor is left with, as a fraction of full (0-1). |

### Power

Power is the risen player's bar. It replaces hunger and sets the size of
the horde.

| Setting | Default | What it does |
|---|---|---|
| `PowerOnRise` | `0.3` | Fraction of a full bar (0-1) a newly risen player starts with. |
| `PowerDrainMinutes` | `45.0` | Minutes for a full bar to drain to empty. At least 1. |
| `PowerPerHit` | `0.01` | Fraction of the bar gained for each of the player's own blows that lands. |
| `PowerKillHuman` | `0.15` | Fraction of the bar gained when the player or their horde kills a survivor. |
| `PowerKillAnimal` | `0.05` | Fraction of the bar gained when the player or their horde kills an animal. |
| `EmpoweredPower` | `80` | At this much power (0-100) or more the jog becomes the knuckle run. Above `100` switches it off. |

### Feasting

| Setting | Default | What it does |
|---|---|---|
| `FeastBiteSeconds` | `2.0` | Seconds per bite. At least 0.2. |
| `FeastHealth` | `4.0` | Health every bite restores. |
| `FeastBlood` | `100.0` | Blood every bite restores. |
| `FeastBitesPerCorpse` | `10` | Bites a dead survivor holds. |
| `FeastPowerHuman` | `0.06` | Fraction of the power bar one bite of a dead survivor gives. |
| `FeastBitesPerAnimal` | `5` | Bites a dead animal holds. |
| `FeastPowerAnimal` | `0.04` | Fraction of the power bar one bite of a dead animal gives. |
| `FeastBitesPerSmallAnimal` | `2` | Bites a dead small animal holds. |
| `FeastPowerSmallAnimal` | `0.03` | Fraction of the power bar one bite of a small animal gives. |
| `FeastPowerHumanMeat` | `0.08` | Fraction of the power bar a human steak gives. |

### Abilities

| Setting | Default | What it does |
|---|---|---|
| `CooldownSwipeMs` | `1500` | Milliseconds between Swipes (key 1). At least 250. |
| `CooldownPoundMs` | `3500` | Milliseconds between Pounds (key 2). At least 250. |
| `CooldownClawMs` | `6000` | Milliseconds between Claws (key 3). At least 250. |
| `CooldownRallyMs` | `20000` | Milliseconds between Rallies (key 4). At least 1000. |
| `CooldownMarkMs` | `10000` | Milliseconds between Marks (key 5). At least 250. |
| `CooldownScentMs` | `30000` | Milliseconds between uses of Scent (key 6). At least 1000. |
| `DamageSwipe` | `"MeleeInfected"` | Damage profile Swipe uses: a `CfgAmmo` class name. |
| `DamagePound` | `"MeleeInfected_Heavy"` | Damage profile Pound uses. |
| `DamageClaw` | `"MeleeInfected"` | Damage profile Claw uses. |
| `ClawBloodLoss` | `300.0` | Extra blood a clawed victim loses. |
| `MarkRange` | `200.0` | How far away, in metres, a target can be marked. |
| `ScentSeconds` | `8` | Seconds Scent lasts. 1 to 60. |
| `ScentRangeMin` | `60.0` | Reach of Scent in metres at no power. |
| `ScentRangeMax` | `200.0` | Reach of Scent in metres at full power. |

### Horde

| Setting | Default | What it does |
|---|---|---|
| `HordeMax` | `30` | Horde size at a full power bar. |
| `HordeMin` | `2` | Horde size at a nearly empty power bar. |
| `RallyRadius` | `100.0` | Infected within this many metres answer a rally. |
| `HordeAttackRange` | `150.0` | Attack order: how far ahead, in metres, the horde looks for prey. |
| `HordeChargeDistance` | `60.0` | Attack order: how far, in metres, the horde charges when there is no prey. |
| `HordePatrolRadius` | `30.0` | Radius of the patrol circuit, in metres. |
| `HordeStragglerSeconds` | `40.0` | A member that makes no progress for this many seconds is released. At least 5. |
| `FeralSeconds` | `60.0` | Seconds a horde runs wild after its leader is killed. |
| `HordeFeastMax` | `4` | How many of the horde gather round and feed with their leader. 0 to 6; `0` = none. |
| `HordeCanBeEquipped` | `1` | Let a risen player dress the infected of their horde. With their inventory open, the members within reach are listed beside them and take headgear, eyewear, a vest and a pack; they stand still meanwhile. What a member wears protects it, and drops with its body. |

### Toughness

| Setting | Default | What it does |
|---|---|---|
| `RisenBodyDamage` | `1.0` | How much of a hit anywhere but the head a risen player takes (0-1). `1`: they are hurt as a survivor is. `0.5`: body and limb hits do half damage. `0`: only hits to the head hurt them. Falls, hunger and the like are not hits and always count. |

### Doors

| Setting | Default | What it does |
|---|---|---|
| `RisenCanBreakDoors` | `1` | Let risen players break down closed and locked doors. They cannot open doors any other way, so with this off a closed door stops them. |
| `HordeCanBreakDoors` | `1` | Let the infected of a horde break through doors in their way. |
| `DoorHitSeconds` | `0.9` | Seconds per blow. At least 0.2. |
| `DoorHits` | `7` | Blows a door takes. At least 1. |

### Voice

| Setting | Default | What it does |
|---|---|---|
| `RisenVoice` | `1` | What others hear when a risen player talks. `0` normal speech, `1` mumbling (the game's effect for a gagged player), `2` nothing. Risen players always hear survivors normally. |

### Diagnostics

| Setting | Default | What it does |
|---|---|---|
| `DebugLog` | `0` | Log rallies, orders, feasting and horde stragglers to the script log. |

### Examples

Every death turns the player:

```json
"RiseChance": 1.0,
"RiseChanceOtherDeaths": 1.0,
```

Half of those killed by the infected rise, nobody else:

```json
"RiseChance": 0.5,
"RiseChanceOtherDeaths": 0.0,
```

Only a hit to the head hurts a risen player:

```json
"RisenBodyDamage": 0.0,
```

Doors stop the risen and their hordes:

```json
"RisenCanBreakDoors": 0,
"HordeCanBreakDoors": 0,
```

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

Play the mod and run it on your servers by subscribing on the
[Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=3839248989). Do not repack it, re-upload it or modified versions of it, include
its files in another mod or server pack, or sell it or access to it. The
full terms are in `LICENSE.txt`.
