# Running 'The Front' Linux server

1. [BEWARE, HERE BE DRAGONS](#beware-here-be-dragons)
1. [Installing](#installing)
1. [Starting the server](#starting-the-server)
1. [Keeping it running](#keeping-it-running)
1. [Configuration](#configuration)
1. [Shutting down the server](#shutting-down-the-server)
1. [Wiping the server](#wiping-the-server)
1. [Admin Commands](https://docs.google.com/spreadsheets/d/1Cea87x09rWuKjKuaqbMBSFdZjXigP5AkcVrT345RMfE/edit#gid=0)
2. [Possible Config Options](#possible-config-options)
3. [Server Tags](#server-tags)
4. [Admins and Admin Levels](#admins-and-admin-levels)
1. [Console](#console)
1. [Logs](#logs)

## BEWARE, HERE BE DRAGONS

**PLEASE READ THIS BIT CAREFULLY**

This repo is purely for my benefit, the files below work for me, I'm not saying they'll work for you but they might or they might give you ideas... who knows.

These scripts were written and are running on an Ubuntu 22.04 Server installation, on a VPS.  I haven't myself tried them running in a VM but can't see why it wouldn't work, possibly with some tweaking.

If you use these and your server breaks well such is life, don't copy/paste things you find on t'intarmawebs.

Support for these files is limited, if you don't understand them or how to fix things then there was this thing invented a while ago that sort of allows you to look around the t'intarmawebs and find information, amazing thing.  It's called Google, search for it and use it.

I'll help if I can, but don't expect issues and questions answered with any sort of timeliness.

---

## Installing

### Getting the correct files, initially

Create a shell script called `updateTheFront.sh` with these contents:

```bash
#! /usr/bin/bash

export SERVERDIR=$HOME/TheFront/server # change this to be where you want the server to live

echo "Downloading The Front Linux Server files..."
steamcmd +force_install_dir $SERVERDIR +login anonymous +app_update 1007 +app_update 2334200 validate  +quit

```

You must change `$SERVERDIR` to point at where you want the server to live.  The actual place doesn't have to be within the normal structure of a Steam install, I prefer to keep mine separate as the game doesn't care too much where it is.

Run this script, and you should have a folder in the place specified as `$SERVERDIR` containing the game files.

---

### Starting the server

Create a shell script called `theFront.sh` with these contents.

```bash
#! /usr/bin/bash

# directories
export USERDIR=$HOME/TheFront/data
export SERVERDIR=$HOME/TheFront/server

mkdir -p $USERDIR
mkdir -p $SERVERDIR

steamcmd +force_install_dir $SERVERDIR +login anonymous +app_update 1007 +app_update 2334200 validate  +quit

# descriptions
export SERVERNAME="Server name goes here"
export SERVERTITLE="Server description goes here" # not used currently

# next wipe
export SERVERRESETDATE=2023-11-24

# server password
export SERVERPASSWORD="SuperSecretPassword"

# server admins
export SERVERADMINS="7656xxxxxxxxxxxxx"

# server external ip
export SERVERIP=31.x.x.x

# ports
export GAMEPORT=5001
export BEACONPORT=5002
export QUERYPORT=5003
export SHUTDOWNPORT=5004

# pve/pvp
export FIGHTMODE=1 # 1=pve, 0=pvp

# maxplayers
export MAXPLAYERS=40

# save how often in seconds
export SAVEINTERVAL=300

# use ACE anti-cheat?
export USEANTICHEAT=true

# should chat be censored? 
export ALLOWSENSITIVEWORDS=true # false=censored, true=not censored

# change to $SERVERDIR
cd $SERVERDIR

./ProjectWar/Binaries/Linux/TheFrontServer \
    ProjectWar_Start?DedicatedServer?MaxPlayers=$MAXPLAYERS \
    -QueueThreshold=$MAXPLAYERS \
    -ServerName="$SERVERNAME" -ServerAdminAccounts="$SERVERADMINS" \
    -SteamServerName="$SERVERNAME" -ServerPassword="$SERVERPASSWORD"\
    -log -locallogtimes \
    -EnableParallelCharacterMovementTickFunction -EnableParallelCharacterTickFunction \
    -UseDynamicPhysicsScene -OutIPAddress=$SERVERIP \
    -ServerID=ANY_IDEA -port=$GAMEPORT \
    -BeaconPort=$BEACONPORT -QueryPort=$QUERYPORT \
    -Game.PhysicsVehicle=true \
    -Game.MaxFrameRate=35 -ShutDownServicePort=$SHUTDOWNPORT \
    -ServerFightModeType=$FIGHTMODE \
    -IsCanSelfDamage=true -IsCanFriendDamage=true \
    -MaxQueueSize=$MAXPLAYERS -QueueValidTime=60 \
    -SaveWorldInterval=$SAVEINTERVAL -GreenHand=true \
    -ClearSeverTime=$SERVERRESETDATE \
    -SensitiveWords=$ALLOWSENSITIVEWORDS -UseACE=$USEANTICHEAT \
    -UserDir=$USERDIR

```
#### **Important**

If you don't require a password on your server then you **MUST** completely remove the `-ServerPassword` line.  Passing an empty password to the server will prompt for a password that you can't enter and will **NOT** allow any logins to the server.

#### Running on a network using NAT (P2P)

If you are running this on a local (at home) computer, then you need to change the start script slightly.

Change your equivalent of this line:

`ProjectWar_Start?DedicatedServer?MaxPlayers=$MAXPLAYERS`

to something like this:

Steam:
`ProjectWar_Start?DedicatedServer?MaxPlayers=$MAXPLAYERS?udrs=steam` 

Epic:
`ProjectWar_Start?DedicatedServer?MaxPlayers=$MAXPLAYERS?udrs=eos`



#### Finally

This script will run the server with the info specified in the environment variables at the top of the script.  However it will stop when you log out and that's not much use eh?

---

### Keeping it running

Create yet another shell script called `startTheFront.sh` with these contents:

```bash
#! /usr/bin/bash
tmux new-session -d -s theFront -n front
tmux send-keys -t theFront:front "$HOME/theFront.sh" Enter
tmux attach -t theFront:front
```

This will start the server in tmux (a terminal multiplexer), you can find information about it [in the tmux wiki](https://github.com/tmux/tmux/wiki).

When logging out, use the `Ctrl-B D` command to detach from the tmux session, rather than Ctrl-C which will stop the server.

When you ssh into the server next time, just run `tmux attach` to re-attach to the running session.

---

### Configuration

Using $SERVERDIR above as a base the configuration file `ServerConfig_.ini` lives in $SERVERDIR/TheFrontManager/ServerConfig_.ini.  If $SERVERDIR/TheFrontManager doesn't exist then create that folder and run this command to create the file required:

```bash
echo '[BaseServerConfig]
IsCanMail=1' > ServerConfig_.ini
```

Customize this with variables to get the server how you'd like it.

For example, to set the `PlayerAddExpRate` add this line in the ServerConfig_.ini file, like so:

```ini
[BaseServerConfig]
IsCanMail=1.000000
PlayerAddExpRate=1.500000
```
---

### Shutting down the server

The server needs to be closed with a single Ctrl-C to initiate a final save before close down (a graceful shutdown).

The server also opens a port, `-ShutDownServicePort=5004` in the script above, which allows a connection via telnet to shutdown gracefully.

Here is a small script which will programatically drive a telnet connection to gracefully shutdown the server:

```bash
#! /usr/bin/expect

set timeout 5

set host localhost
set port 5004

spawn telnet $host $port

expect "Welcome to server\n\n"

send "shutdown\r"
```

---

### Wiping the server

To completely wipe the server you need to remove the files listed below:

```bash
$SAVEDIR/Saved/ListenServer/
$SAVEDIR/Saved/Logs/ 
$SAVEDIR/Saved/GameStates/Accounts/Accounts.csv
$SAVEDIR/Saved/GameStates/Accounts/Accounts.csv.back
$SAVEDIR/Saved/GameStates/Accounts/NickNames.csv
$SAVEDIR/Saved/GameStates/DeletedPlayers/
$SAVEDIR/Saved/GameStates/Worlds/
$SAVEDIR/Saved/GameStates/Players/
$SAVEDIR/Saved/GameStates/ConstructData.sav
$SAVEDIR/Saved/GameStates/GuildData.sav 
```

You can leave the logs, or move them out of the way, if you want a record of them.

You might be able to get away with keeping GuildData.sav if you want to keep squads, not tested thoroughly.

The file `$SAVEDIR/Saved/GameStates/Accounts/GM.csv` contains info on admins and bans.  You can leave this alone if you want to keep the bans in place.

Thanks to ScareCr0w12 for this information on wipes.

---

### Possible Config Options

These are the config options that are pumped out from TheFrontManager, there may be others that are missing here:

```ini
ServerName=
ServerPassword=
QueueThreshold=40
ServerFightModeType=1
IsCanSelfDamage=0
IsCanFriendDamage=0
ClearSeverTime=2023-10-11
UseSteamSocket=1
Port=5001
BeaconPort=5002
ShutDownServicePort=5003
SaveWorldInterval=300
GMOverlapRatio=1
IsUnLockAllTalentAndRecipe=0
GMBagInitGirdNum=40
GreenHand=1
CharacterInitItem=
GMDeathDropMode=1
GMDeathInventoryLifeSpan=1800
CorpsePickAuthority=2
GMCanDropItem=1
GMCanDiscardItem=1
GMDiscardBoxLifeSpan=300
GMRebirthBaseCD=10
GMRebirthExtraCD=1
GMPenaltiesMaxNum=5
GMPenaltiesCD=600
ConstructEnableRot=1
GMAttackCityCdRatio=1
OpenAllHouseFlag=0
IsCanChat=1
IsShowBlood=1
SensitiveWords=1
HealthDyingState=1
UseACE=1
ServerAdminAccounts=
IsShowGmTitle=0
PlayerHotDefAddRate=1
PlayerIceDefAddRate=1
HeadNameDisplayDist_Team=200
HeadNameDisplayDist_Enemy=20
PlayerDeathAvatarItemDurableRate=0
PlayerDeatShortcutItemDurableRate=0
GMCraftTimeRate=1
PlayerAddExpRate=1
PlayerKillAddExpRate=1
PlayerFarmAddExpRate=1
PlayerCraftAddExpRate=1
MoveSpeedRate=1
JumpRate=1
PlayerLandedDamageRate=1
PlayerMaxHealthRate=1
HealthRecoverRate=1
PlayerMaxStaminaRate=1
StaminaRecoverRate=1
PlayerStaminaCostRate=1
PlayerMaxHungerRate=1
GMHungerDecRatio=1
GMBodyHungerAddRate=1
MaxBodyWaterRate=1
GMWaterDecRatio=1
GMBodyWaterAddRate=1
MaxBreathRate=1
BreathRecoverRate=1
PlayerBreathCostRate=1
GMPlayerHealthRate=1
GMFoodDragDurationRate=1
NpcRespawnRatio=1
AnimalBodyStayTime=300
HumanBodyStayTime=10
GMNPCLootableItemRatio=1
NpcSpawnLevelRatio=1
WildNPCDamageRate=1
WildNPCHealthRate=1
WildNPCSpeedRate=1
CityNPCLevelRate=1
CityNPCDamageRate=1
CityNPCHealthRate=1
CityNPCSpeedRate=1
CityNPCNumRate=1
NpcDisplayDistance=50
GMInventoryGainRate=1
GMCityATKNPCLootItemRatio=1
GMMaxHouseFlagNumber=1
GMSetGJConstructMaxNumRatio=1
GMHFTrapMaxNum=0
GMHFTurretMaxNum=0
GMConstructDefenseRatio=1
GMTrapDefenseRatio=1
GMTurretDefenseRatio=1
GMTrapDamageRatio=1
GMTurretDamageRatio=1
GMConstructMaxHealthRatio=1
GMConstructReturnHPRatio=1
GMHouseFlagRepairHealthRatio=1
GMTTC_Oil_Rate=1
GMWaterCollecter_Rate=1
GMTTC_Ore_Rate=1
GMTTC_Fish_Rate=1
CHFDamagedByPlayer=1
CHFDamagedByVehicle=1
CHFDamagedByNpc=1
GMHouseFlagExcitantTime=3
GMMaxRetrieveProductsRate=1
GMTreeGainRate=1
GMBushGainRate=1
GMOreGainRate=1
GMCropVegetableReapRatio=1
GMFleshGainRate=1
GMCropVegetableGrowRatio=1
GMMeleeNpcDamageRatio=1
GMRangedNpcDamageRatio=1
GMMeleePlayerDamageRatio=1
GMRangedPlayerDamageRatio=1
GMMeleeConstructDamageRatio=1
GMRangedConstructDamageRatio=1
GMToolDamageRate=1
GMDurabilityCostRatio=1
GMVehiclePlayerDamageRatio=1
GMVehicleConstructDamageRatio=1
GMVehicleDamageRate=1
```
---

### Server Tags

When your server is shown in the in-game server browser you can add tags to your server display to show how it is configured.

![image](https://github.com/pharrisee/TheFrontLinuxServerInfo/assets/121816/db351753-2ead-45f7-800e-cf17c20e736c)

This is done by adding a `-ServerTags` option to your start script, e.g.:

```
-ServerTags=1,2,3,4,8
```

The above would add tags that show that the server is PVE; has a multiplier for XP; has raised gather rate; you keep inventory on death; and a 60 day wipe.

Use these values to add your tags:
| Tag | Meaning |
|---|---|
|0| PVP |
|1| PVE|
|2| EXP Multiplier|
|3| Gather rate|
|4| Keep Inventory|
|5| 45d wipe|
|6| 15d wipe|
|7| 30d wipe|
|8| 60d wipe|

Currently there is a bug whereby it will always show `*1` as the rate regardless of the actual rate you have configured for the various options.

---

### Admins and Admin Levels

You can add a few different levels of admin (GM) to your server with the command `AddGM STEAM:steamid <level>` and remove them with `RemoveGM STEAM:steamid`, e.g.:

```ini
AddGM STEAM:7656xxxxxxxxxxxxx 25
```

or 

```ini
RemoveGM STEAM:7656xxxxxxxxxxxxx
```

The levels are defined as follows:

|Level|Meaning|
|---|---|
|20| Priority queue|
|21| See GM Console, Able to teleport|
|22| Able to see player list in full|
|23| Able to kick and kill|
|24| able to spawn items|
|25| All abilities|

---

### Console

The console can be opened by pressing the backtick key `` ` ``.  This opens a very small single line console at the bottom of the screen.  Pressing the backtick twice opens a full console.

Some keyboard layouts have problems opening the console, Scandinavian and German layouts are known to cause issues and probably others as well.  One 'fix' for this is to change keyboard layout to a UK layout whilst in game (temporarily) and open the console whilst that layout is in force.

---

### Logs

Log files for the server can be found at `$SAVEDIR/Saved/Logs`.  They are not currently overly helpful as they are both too detailed and at the same the not containing the info you might require for GM'ing a server.