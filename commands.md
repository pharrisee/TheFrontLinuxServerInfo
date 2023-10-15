# Console Commands
## Server Player Limit
The max number of players on your server.

```
SetQueueThreshold [amount]  SetMaxQueueSize [amount]
```
## Damage self?
Allows players to damage themselves.

```SetIsCanSelfDamage 0(Disable)/1(Enable)```

## Damage allies?
Allows squadmates to damage each other.

```SetIsCanFriendDamage 0 (Disable)/1 (Enable)```

## Allow chat (incl. voice)?
When disabled, prevents players from sending chat messages in-game.

```SetCanChat 1 (Allowed) 0 (Not allowed)```

## Allow attachments?
When disabled, prevents receiving attachments in-game.

```MailAttchEnable 0 (Not allowed)/1 (Allowed)```

## Server save interval
Server archive interval (in seconds)

```SetSaveGameInterval [seconds]```

## Item stack rate
Stack limit for each type of item.

```GMSetOverlapRatio [multiplier]```

## Drop items on death?
0 = No drops, 1 = Drop all, 2 = Drop inventory (does not include equipped or hotkey items)

```GMSetDeathDropMode [parameter]```

## Set structure decay
When enabled, structures will decay.
``` SetConstructDisableRot 1 (Disabled) 0 (Enabled)```

## Creatures/structures drop items on death?
When enabled, creatures and structures will drop items when killed/destroyed.

```GMSetCanDropItem 0= No drops; 1= All drop```

## Allow item discard?
Set whether players can discard items.

``` GMSetCanDiscardItem 0 (cannot discard)/1 (can discard)```

## Enable wounded state?
Enables whether wounded state is triggered when HP falls to 0, or player immediately dies.

```SetPlayerHealthDyingState 1 (Wounded state is enabled)/0 (Wounded state is disabled)```

## Add admin account
Enter a 17-digit Steam ID. Use semicolons between each ID. GM level defaults to highest level (Lv. 25)

```AddGM [Account ID] [GM level]```

## Remove admin account
Removes an admin. Format: AccountID
``` RemoveGM [AccountID]```

## Label admin
Toggles special admin icon.

```ToggleGMTitleShow 0 (Not displayed)\1 (Displayed)```

## Discarded items despawn
Items discarded will disappear after this amount of time.

```GMSetDiscardBoxLifeSpan [time (in seconds)] (Default -1 to use default settings)```

## Dropped items despawn after death
Items dropped on death will disappear after this amount of time.
```GMSetDeathInventoryLifeSpan [time (in seconds)] (Default -1 to use default settings)```

## Monster raid cooldown rate
Multiplies the cooldown time between supply deliveries.

```SetAttackCityCdRatio```

## Initial respawn cooldown
Basic revival cooldown duration.

```SetGMRebirthBaseCD [time (in seconds)]```

## Respawn cooldown penalty
Amount by which revival cooldown increases after multiple deaths.

```SetGMRebirthExtraCD [time (in seconds)]```

## Death penalty times
#N/A

```SetGMPenaltiesMaxNum [times]```

## Death penalty respawn time
Time after which stacked revival cooldowns are reset.

```SetGMPenaltiesCD [time (in seconds)]```

## Show all Spacetime Beacons
Displays the location of other players' Beacons on the map.

```OpenAllHouseFlag 0 (Disabled)/1 (Enabled)```

## Heat RES Boost rate
Multiplies character Heat RES.

```SetPlayerHotDefAddRate [multiplier]```

## Cold RES Boost rate
Multiplies character Cold RES.

```SetPlayerIceDefAddRate [multiplier]```

## Friendly name display distance
Max distance at which you can see a squadmate's name (in meters)

```SetFriendDisplayDistance [distance (in meters)]```

## Non-friendly name display distance
Max distance at which you can see a non-squadmate's name (in meters)

```SetEnemyDisplayDistance [distance (in meters)]```

## Inventory item DUR death penalty
On death, the Durability of equipped items will fall by max Durability times this amount (does not affect drops)

```SetPlayerDeathAvatarItemDurableRate [multiplier]```

## Hotbar item DUR death penalty
On death, the Durability of hotbar items will fall by max Durability times this amount (does not affect drops)
```SetPlayerDeatShortcutItemDurableRate [multiplier]```

## Crafting/repair time rate
Multiplies item crafting/repair time.

```GMSetCraftTimeRate [multiplier]```

## All XP rate
Multiplies all XP earned.

```SetPlayerAddExpRate [multiplier]```

## Kill XP rate
Multiplies XP earned from killing monsters

```SetPlayerKillAddExpRate [multiplier]```

## Collection XP rate
Multiplies XP earned from collecting resources

```SetPlayerFarmAddExpRate [multiplier]```

## Crafting XP rate
Multiplies XP earned from crafting items

```SetPlayerCraftAddExpRate [multiplier]```

## Movement speed rate
Multiplies movement speed

```SetMoveSpeedRate [multiplier]```

## Jump height rate
Multiplies jump height

```SetJumpHeightRate [multiplier]```

## Fall DMG boost
Multiplies damage taken from falls.

```SetPlayerLandedDamageRate [multiplier]```

## HP rate
Multiplies max HP.

```SetPlayerMaxHealthRate [multiplier]```

## HP recovery rate
HP recovery will be multiplied by this value.

```SetLifeRecoverRate [multiplier]```

## Stamina rate
Multiplies max stamina.

``SetPlayerMaxStaminaRate [multiplier]```

## Stamina recovery rate
Stamina recovery will be multiplied by this value.

```SetStaminaRecoverRate [multiplier]```

## Stamina loss rate
Multiplies stamina consumption speed.

```SetStaminaConsumeRatio [multiplier]```

## Max fullness multiplier
Max Fullness will be multiplied by this value.

```SetPlayerMaxHungerRate [multiplier]```

## Fullness loss rate
Multiplies Fullness consumption speed.

```GMSetHungerDecRate [multiplier]```

## Fullness from food rate
Multiplies amount of Fullness recovered by eating.

```GMSetBodyHungerAddRate [multiplier]```

## Max hydration multiplier
Max Hydration will be multiplied by this value.

```SetBodyWaterMaximumRate [multiplier]```

## Hydration loss rate
Multiplies Hydration consumption speed.

```GMSetWaterDecRate [multiplier]```

## Hydration from water rate
Multiplies amount of Hydration recovered by drinking.

```GMSetBodyWaterAddRate [multiplier]```

## Max O2 multiplier
Multiplies max Oxygen.

```SetBreathMaximumRate [multiplier]```

## O2 recovery rate
Max Oxygen will be multiplied by this value.

```SetBreathRecoverRate [multiplier]```

## O2 loss speed
Multiplies Oxygen consumption speed.

```SetPlayerBreathCostRate [multiplier]```

## Med recovery rate
Multiplies amount of HP recovered from meds.

```GMSetPlayerHealthRate [multiplier]```

## Food and med duration rate
Multiplies the duration of effect of food and meds.

```GMSetFoodDragDurationRate [multiplier]```

## Vehicle vs. player DMG rate
Multiplies the amount of damage vehicles deal to players.

```SetVehiclePlayerDamageRatio [multiplier]```

## Vehicle vs. structure DMG rate
Multiplies the amount of damage vehicles deal to structures.

```SetVehicleConstructDamageRatio [multiplier]```

## Vehicle collection rate
Multiplies the amount of resources collected by vehicles.

```GMSetVehicleDamageRate [multiplier]```

## All monster respawn time rate
Multiplies time before dead wild NPCs can respawn.

```SetNpcRespawnRate [multiplier]```

## Monster NPC corpse despawn time
Animal NPC corpses will disappear after this amount of time (in seconds)

```SetAnimalBodyStayTime [time (in seconds)]```

## Human NPC corpse despawn time
Human NPC corpses will disappear after this amount of time (in seconds)

```SetHumanBodyStayTime [time (in seconds)]```

## Wild NPC corpse drop rate
Multiplies the amount of items dropped by a wild NPC on death.

```GMSetNPCLootableItemRate [multiplier]```

## Wild NPC starting lv. rate
Multiplies the level of wild NPCs, making them more powerful.

```SetNpcSpawnLevelRate [multiplier]```

## Wild NPC DMG rate
Multiplies the damage dealt by wild NPCs.

```SetWildNPCDamageRate [multiplier]```

## Wild NPC HP rate
Multiplies the HP of wild NPCs.

```SetWildNPCHealthRate [multiplier]```

## Wild NPC speed rate
Multiplies the movement speed of wild NPCs.

```SetWildNPCSpeedRate [multiplier]```

## Raid NPC lv. rate
Multiplies the level of raid NPCs, making them more powerful.

```SetCityNPCLevelRate [multiplier]```

## Raid NPC DMG rate
Multiplies the damage dealt by raid NPCs.

```SetCityNPCDamageRate [multiplier]```

## Raid NPC HP rate
Multiplies the HP of raid NPCs.

```SetCityNPCHealthRate [multiplier]```

## Raid NPC speed multiplier
Multiplies the movement speed of raid NPCs.

```SetCityNPCSpeedRate [multiplier]```

## Raid NPC amt rate
Multiplies the total amount of raid NPCs summoned each round.

```SetCityNPCNumRate [multiplier]```

## NPC name display distance
Max distance at which you can see an NPC's name (in meters)

```SetNpcDisplayDistance [distance]```

## Collectible resource spawn rate
Multiplies the amount of resources received upon collection.

```GMSetInventoryGainRate [multiplier]```

## Raid corpse drop rate
Multiplies the amount of items dropped by a raid NPC on death.

```GMSetCityAtkNPCLootItemRate [multiplier]```

## Melee DMG vs. NPC rate
Multiplies melee weapon damage that characters deal to NPCs.

```SetMeleeNpcDamageRatio [multiplier]```

## Ranged DMG vs. NPC rate
Multiplies ranged weapon damage that characters deal to NPCs.

```SetRangedNpcDamageRatio [multiplier]```

## Melee PVP DMG rate
Multiplies melee weapon damage that characters deal to players.

```SetMeleePlayerDamageRatio [multiplier]```

## Ranged PVP DMG rate
Multiplies ranged weapon damage that characters deal to players.

```SetRangedPlayerDamageRatio [multiplier]```

## Melee DMG vs. structures rate
Multiplies melee weapon damage that characters deal to structures.

```SetMeleeConstructDamageRatio [multiplier]```

## Ranged DMG vs. structures rate
Multiplies ranged weapon damage that characters deal to structures.

```SetRangedConstructDamageRatio [multiplier]```

## Collection tool DMG multiplier
Multiplies damage that tools deal to resources.

```GMSetToolDamageRate [multiplier]```

## Durability loss multiplier
Multiplies durability lost when using tools, weapons, and armor.

```GMSetDurabilityCostRate [multiplier]```

## Resource collection rate
Controls max gains from collection. This value must be bigger than other collection rates.

```GMSetMaxRetrieveProductsRate [max multiplier] (Default -1 means no upper limit is determined)```

## Wood production rate
Multiplies amount of wood collected.

```GMSetTreeGainRate [multiplier]```

## Plant production rate
Multiplies amount of plants collected.

```GMSetBushGainRate [multiplier]```

## Ore production rate
Multiplies amount of ore collected.

```GMSetOreGainRate [multiplier]```

## Crop production rate
Multiplies amount of crops collected.

```GMSetCropReapRate [multiplier]```

## Animal production rate
Multiplies amount of items collected from corpses.

```GMSetFleshGainRate [multiplier]```

## Crop growth speed
Multiplies growth speed of planted crops.

```GMSetCropGrowRate [multiplier]```

## Player Beacon limit
Max number of Beacons each player can build.

```SetPlayerMaxHouseFlagNumber```

## Player work structure limit
Max number of work-type structures each player can build will be multiplied by this value.

```SetGJConstructMaxNumRatio [multiplier]```

## Beacon trap limit
Max number of traps that can be within Beacon coverage.

```SetHFTrapMaxNum [amount]```

## Beacon turret limit
Max number of turrets that can be within Beacon coverage.

```SetHFTurretMaxNum [amount]```

## Structure DMG taken rate (non-traps, turrets, beacons)
Damage taken is multiplied by this value.

```SetConstructDefenseRatio [ratio]```

## Trap DMG taken rate
Damage taken is multiplied by this value.

```SetTrapDefenseRatio [ratio]```

## Turret DMG taken rate
Damage taken is multiplied by this value.

```SetTurretDefenseRatio [ratio]```

## Trap DMG rate
Multiplies damage dealt by traps.

```SetTrapDamageRatio [multiplier]```

## Turret DMG rate
Multiplies damage dealt by turrets.

```SetTurretDamageRatio [multiplier]```

## Structure DUR rate
Multiplies the max Durability of structures.

```SetConstructMaxHealthRatio [multiplier]```

## Structure DUR recovery rate
Multiplies the Durability recovery speed of placed structures.

```SetConstructReturnHPRatio [multiplier]```

## Spacetime Beacon auto-repair rate
Multiplies the Durability recovery speed of structures within auto-repair range of a Beacon.

```SetHouseFlagRepairHealthRatio [multiplier]```

## Oil Well work production rate
Multiplies resources collected from Oil Wells.

```GMSetTTC_Oil_Rate [multiplier]```

## Dew Collector work production rate
Multiplies resources collected from Dew Collectors.

```GMSetWaterCollector_Rate [multiplier]```

## Mine work production rate
Multiplies resources collected from Mines.

```GMSetTTC_Ore_Rate [multiplier]```

## Fish Basket work production rate
Multiplies resources collected from Fish Baskets.

```GMSetTTC_Fish_Rate [multiplier]```

## Beacon player gun DMG RES rate
Damage that beacons take from player firearms is multiplied by this value.

```SetCHFDamagedRateByPlayer [ratio]```

## Beacon player vehicle DMG RES rate
Damage that beacons take from player vehicles is multiplied by this value.

```SetCHFDamagedRateByVehicle [ratio]```

## Beacon NPC DMG RES rate
Damage that beacons take from NPCs is multiplied by this value.

```SetCHFDamagedRateByNpc [ratio]```

## Beacon daily non-protection duration
Sets the amount of time that beacons can be attacked. Values must be between 0 and 24.

```SetHouseFlagExcitantTime [number]```

## Close server
Close server

```CloseServer```

## Save world
Saves server data

```SaveWorld```

## Set world time
#N/A

```SetTime [time value]```

## Set weather
#N/A

```SetWeather [weather ID] [region ID]```

## Enable/disable invisibility
Renders monster, player, NPC, or defensive structure invisible.

```hide 1(Enabled)/0 (Disabled)```

## Enable/disable flight
There are no walking animations or sound effects while in flight.

```Fly/Walk```

## Set Speed
#N/A

```SetAttribute 36 (attribute number) [value]```

## Enable/disable noclip
Enables flight and removes character collision, allowing them to pass through objects.

```Ghost```

## Enable/disable structure sight
Displays structure names and structure owner names, making it easier to find buildings on the map.

```PerspectiveConstruct 0 (don't display)/1 (display)```

## Enable/disable player sight
Displays player names, making it easier to find players.

```PerspectivePlayer 0 (don't display)/1 (display)```

## Go to
Command + coordinates

```goto x y z```

## Kill all spawned NPCs
#N/A

```ClearAllNPC```

## Set own model size
Changes the size of your character model.

```SetPlayerScaleRate [scale]```

## Disable all invulnerability
Affects all players on the current server.

```ClearAllPlayersGodMode```

## Enable/disable infinite stamina
#N/A

```ActivateInfiniteStamina 1 (Enabled)/0 (Disabled)```

## Enable/disable environmental immunity
#N/A

```ActivateIgnoreEnvironment 1(Enabled)/0 (Disabled)```

## Enable/disable infinite HP
Recovers Max HP every second.

```ActivateInfiniteRecoverHealth 1 (Enabled)/0 (Disabled)```

## Kill target squad's players, creatures, and vehicles, and destroy all its structures (with drops)
If target player is not on a squad, only they will be affected. If target player is on a squad, the whole squad will be affected. Upon death/destruction, items will be dropped.

```KillGuildAll 1```

## Kill target squad's players, creatures, and vehicles, and destroy all its structures (no drops)
If target player is not on a squad, only they will be affected. If target player is on a squad, the whole squad will be affected. Upon death/destruction, items will not be dropped.

```KillGuildAll 0```

## Destroy target squad's structures within radius of 1-150 (with drops)
Command + radius parameter. If target player is not on a squad, only they will be affected. If target player is on a squad, the whole squad will be affected.

```KillRadiusGuildConstruct 1 [radius (in meters)]```

## Destroy target squad's structures within radius of 1-150 (no drops)
Command + radius parameter. If target player is not on a squad, only they will be affected. If target player is on a squad, the whole squad will be affected.

```KillRadiusGuildConstruct 0 [radius (in meters)]```

## Destroy target squad's vehicles within radius of 1-150
If target player is not on a squad, only they will be affected. If target player is on a squad, the whole squad will be affected.

```KillRadiusGuildVehicle 1 or 0 [radius (in meters)]```

## Clear supply cooldown
Clears target player's supply cooldown.

```ClearAttackCityCD [Player ID]```

## Enable/disable GM Super DMG
Enables one-hit kills on creatures, vehicles, and structures (guns can also destroy structures).

```SetEnableSuperKill```

## Join target structure's squad
Select a squad before entering this command.

```JoinGuild```

## Obtain current squad's max authority
You will become the squad captain, and the original squad captain will be demoted to a member. This function is only for GM operation.

```SetGuildAdmin 1/0```

## Force change squad name
Squad ID + new name; must pass duplicate name detection.

```ForcedChangeGuildName [squad GUID] [new squad name]```

## Force join squad
Command + squad ID (ignores squad player limit).

```JoinGuildByGuid [squad GUID]```

## Kill target unit
Kills unit you are facing (any structure, creature, vehicle).

```KillInteractObject```

## Enable/disable creator mode
Crafting and repairing will not require materials.

```GMCreatorMode Enable state (1 Enabled, 0 Disabled)```

## Toggle overhead view
Enables use of specific shortcut keys to switch to an overhead perspective of a player, at the same time enabling noclip, flight, and invulnerability.

```Shift+C and Shift+V```

## Give item to player (Item ID)
#N/A

```GMAddItems [itemID] [amount] [playerID] (if playerID is not entered, this command will default to yourself)```

## Give EXP to player
Command + Account ID + EXP

```AddTargetPlayerExp [playerID] [EXP value]```

## Spawn specified creature ID (Coordinates)
#N/A

```GMSpawnNPCByLocation [creatureID] [level] [coordinate x] [coordinate y] [coordinate z]```

## Spawn specified creature ID (Character)
#N/A

```GMSpawnNPCByPlayerGuid [creatureID] [level] [amount] [distance (in meters)] [playerID] (if playerID is not entered, this command will default to yourself)```

## Spawn specified pet ID (Character)
#N/A

```GMSpawnPetByPlayerGuid [creatureID] [level] [amount] [distance (in meters)] [playerID] (if playerID is not entered, this command will default to yourself)```

## Go to player
#N/A

```GotoPlayerByAccount [playerID]```

## Bring player to you
#N/A

```RelocatePlayerToGM [playerID]```

## Kill player
#N/A

```DestroyPlayerByGUID [playerID]```

## Clear player items
#N/A

```GMClearInventory [playerID]```

## Unlock all recipes
Command + Account ID

```UnlockTargetAllRecipe [playerID] ("self" to use on self)```

## Unlock all talents
Command + Account ID

```UnlockTargetAllTalent [playerID] ("self" to use on self)```

## Kick offline (no notification)
#N/A

```KickPlayerOff [playerID]```

## Ban user
Prevents player from logging onto the server.

```BanPlayer [playerID] login```

## Unban user
Removes a player's server logon restriction.

```PermitPlayer [playerID] login```

## Mute player (including voice)
Prevents player from using in-game chat function.

```BanPlayer [playerID] chat```

## Unmute player
Remove a player's in-game chat function restriction.
```PermitPlayer [playerID] chat```
