# Running The Front Linux server

1. [BEWARE, HERE BE DRAGONS](#beware-here-be-dragons)
1. [Installing](#installing)
1. [Starting the server](#starting-the-server)
1. [Keeping it running](#keeping-it-running)
1. [Configuration](#configuration)
1. [Shutting down the server](#shutting-down-the-server)
1. [Wiping the server](#wiping-the-server)
1. [Admin Commands](https://docs.google.com/spreadsheets/d/1Cea87x09rWuKjKuaqbMBSFdZjXigP5AkcVrT345RMfE/edit#gid=0)

## BEWARE, HERE BE DRAGONS

**PLEASE READ THIS BIT CAREFULLY**

This repo is purely for my benefit, the files below work for me, I'm not saying they'll work for you but they might or they might give you ideas... who knows.

These scripts were written and are running on an Ubuntu 22.04 Server installation, on a VPS.  I haven't myself tried them running in a VM but can't see why it wouldn't work, possibly with some tweaking.

If you use these and your server breaks well such is life, don't copy/paste things you find on t'intarmawebs.

Support for these files is limited, if you don't understand them or how to fix things then there was this thing invented a while ago that sort of allows you to look around the t'intarmawebs and find information, amazing thing.  It's called Google, search for it and use it.

I'll help if I can, but don't expect issues and questions answered with any sort of timeliness.

## Installing

##### Updated

As of 24-Oct-2023 the "The Front Linux Server" Steam download is ~~broken~~ *fixed*.  ~~What follows is a way to get a Linux server running despite the broken depot.  This script will need to be changed if the depot is ever fixed.~~

~~It seems there is a way to force `steamcmd` to download the correct files by using `+@sSteamCmdForcePlatformType windows` to tell steam to get the linux files correctly.  Making the update Script not only easier to read but much more efficient.  It will only download when there is an actual update.~~

### Getting the correct files

Create a shell script called `updateTheFront.sh` with these contents:

```bash
#! /usr/bin/bash

export SERVERDIR=$HOME/TheFrontServer # change this to be where you want the server to live

echo "Downloading The Front Linux Server files..."
steamcmd +force_install_dir $SERVERDIR +login anonymous +app_update 1007 +app_update 2334200 validate  +quit

```

You must change `$SERVERDIR` to point at where you want the server to live.  The actual place doesn't have to be within the normal structure of a Steam install, I prefer to keep mine separate as the game doesn't care too much where it is.

Run this script, and you should have a folder in the place specified as `$SERVERDIR` containing the game files.

### Starting the server

Create a shell script called `theFront.sh` with these contents.

```bash
#! /usr/bin/bash

# directories
export USERDIR=$HOME/TheFront/data
export SERVERDIR=$HOME/TheFront/server

mkdir -p $USERDIR
mkdir -p $SERVERDIR

steamcmd +force_install_dir $SERVERDIR  +login anonymous +app_update 2334200 +quit

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

If you don't require a password on your server then you **MUST** completely remove the `-ServerPassword` line.  Passing an empty password to the server will prompt for a password that you can't enter and will stop **all** logins.

#### Running on a network using NAT (P2P)

If you are running this on a local (at home) computer, then you need to change the start script slightly.

Change your equivalent of this line:

`ProjectWar_Start`

to something like this:

Steam:
`ProjectWar_Start?udrs=steam` 

Epic:
`ProjectWar_Start?udrs=eos`



#### Finally

This script will run the server with the info specified in the environment variables at the top of the script.  However it will stop when you log out and that's not much use eh?

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
