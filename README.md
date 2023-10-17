# Running The Front Linux server

## BEWARE, HERE BE DRAGONS

The files below work for me, I'm not saying they'll work for you but they might, or they might give you ideas... who knows.

If you use these and your server breaks well such is life, don't copy/paste things you find on t'intarmawebs.

## Installing

As of 14-Oct-2023 the "The Front Linux Server" Steam download is broken.  What follows is a way to get a Linux server running despite the broken depot.  This script will need to be changed if the depot is ever fixed.

The real reason for this is that the depot for the Linux Server (2334201) is marked as a Windows depot rather than a Linux one, so the `app_update` command doesn't correctly get the files.

### Getting the correct files

Create a shell script called `updateTheFront.sh` with these contents:

```bash
#! /usr/bin/bash

export SERVERDIR=$HOME/TheFrontServer # change this to be where you want the server to live

# don't need manifestID as it seems to pick the latest manifest automatically

# download the depot 2334201 (The Front Linux Server)
echo "Downloading The Front Linux Server files..."
steamcmd +login anonymous +download_depot 2334200 2334201 +quit

# make the target directory, if not exists
echo "Creating $SERVERDIR..."
mkdir -p $SERVERDIR

# copy the files from the depot to the target directory
echo "Copying files to $SERVERDIR..."
cp -rv $HOME/.local/share/Steam/steamcmd/linux32/steamapps/content/app_2334200/depot_2334201/* $SERVERDIR
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
export ADMINIDS=7xxxxxxxxxxxxxxxx
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
    -Game.PhysicsVehicle=false \
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

When you ssh into the server next time, just run tmux attach to re-attach to the running session.
