# Running The Front Linux server

1. [BEWARE, HERE BE DRAGONS](#beware-here-be-dragons)
1. [Installing](#installing)
1. [Starting the server](#starting-the-server)
1. [Keeping it running](#keeping-it-running)
1. [Configuration](#configuration)
1. [Wiping the server](#wiping-the-server)
1. [Admin Commands](gm-commands.md)
1. [Config options](config-startup.md)

## BEWARE, HERE BE DRAGONS

**PLEASE READ THIS BIT CAREFULLY**

This repo is purely for my benefit, the files below work for me, I'm not saying they'll work for you but they might or they might give you ideas... who knows.

If you use these and your server breaks well such is life, don't copy/paste things you find on t'intarmawebs.

Support for these files is limited, if you don't understand them or how to fix things then there was this thing invented a while ago that sort of allows you to look around the t'intarmawebs and find information, amazing thing.  It's called Google, search for it and use it.

I'll help if I can, but don't expect issues and questions answered with any sort of timeliness.

## Installing

As of 14-Oct-2023 the "The Front Linux Server" Steam download is broken.  What follows is a way to get a Linux server running despite the broken depot.  This script will need to be changed if the depot is ever fixed.

The real reason for this is that the depot for the Linux Server (2334201) is marked as a Windows depot rather than a Linux one, so the `app_update` command doesn't correctly get the files.

It seems there is a way to force `steamcmd` to download the correct files by using this `+@sSteamCmdForcePlatformType windows` to tell steam to get the linux files correctly.  Making the update Script not only easier to read but much more efficient.  It will only download when there is an actual update.

### Getting the correct files

Create a shell script called `updateTheFront.sh` with these contents:

```bash
#! /usr/bin/bash

export SERVERDIR=$HOME/TheFrontServer # change this to be where you want the server to live

echo "Downloading The Front Linux Server files..."
steamcmd +@sSteamCmdForcePlatformType windows +force_install_dir $SERVERDIR +login anonymous +app_update 2334200 +quit

```

You must change `$SERVERDIR` to point at where you want the server to live.  The actual place doesn't have to be within the normal structure of a Steam install, I prefer to keep mine separate as the game doesn't care too much where it is.

Run this script, and you should have a folder in the place specified as `$SERVERDIR` containing the game files.

### Starting the server

Create a shell script called `theFront.sh` with these contents.

```bash
#! /usr/bin/bash

export SAVEDIR=$HOME/TheFrontServerData
export SERVERDIR=$HOME/TheFrontServer # same as SERVERDIR in the previous script
export SERVERNAME="Server Name goes here"
export SERVERTITLE="For old people by old people!"
# can have multiple admins, separate them by ';'
# these are steamids
export ADMINIDS="7xxxxxxxxxxxxxxxx"
export IPADDRESS=31.x.x.x
export SERVERPASSWORD="a secret password goes here"
export SERVERWIPEDATE=2023-10-20
                    
mkdir -p $SAVEDIR

# change to the server folder specifed above
cd $SERVERDIR

# start the server
./ProjectWar/Binaries/Linux/TheFrontServer \
    ProjectWar_Start?DedicatedServer?MaxPlayers=8? \
    -server \
    -game -QueueThreshold=8 \
    -ServerName="$SERVERNAME" \
    -ServerAdminAccounts=$ADMINIDS \
    -log -locallogtimes \
    -EnableParallelCharacterMovementTickFunction \
    -EnableParallelCharacterTickFunction \
    -UseDynamicPhysicsScene \
    -OutIPAddress=$IPADDRESS \
    -ServerID=ANY_IDEA \
    -port=5001 \
    -BeaconPort=5002 \
    -QueryPort=5003 \
    -Game.PhysicsVehicle=true \
    -ansimalloc \
    -Game.MaxFrameRate=35 \
    -ShutDownServicePort=5004 \
    -ServerPassword="$SERVERPASSWORD" \
    -ServerFightModeType=1 \
    -IsCanSelfDamage=true \
    -IsCanFriendDamage=true \
    -fullcrashdumpalways \
    -MaxQueueSize=2 \
    -QueueValidTime=60 \
    -SaveWorldInterval=300 \
    -GreenHand=true \
    -SensitiveWords=true \
    -UseACE=true \
    -ClearServerTime=$SERVERWIPEDATE \
    -UserDir=$SAVEDIR \
    -ServerTitle="$SERVERTITLE" 

```
#### **Important**

If you don't require a password on your server then you **MUST** completely remove the `-ServerPassword` line.  Passing an empty password to the server will prompt for a password that you can't enter and will stop **all** logins.

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
