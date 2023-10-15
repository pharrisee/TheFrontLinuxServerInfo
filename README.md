# Running The Front Linux server

## Installing

As of 14-Oct-2023 the "The Front Linux Server" Steam download is broken.  What follows is a way to get a Linux server running despite the broken depot.  This script will need to be changed if the depot is ever fixed.

### Getting the correct files

Create a shell script called `updateTheFront.sh` with these contents:

```bash
#! /usr/bin/bash

export targetDir=$HOME/TheFrontServer

# don't need manifestID as it seems to pick the latest manifest automatically

# download the depot 2334201 (The Front Linux Server)
echo "Downloading The Front Linux Server files..."
steamcmd +login anonymous +download_depot 2334200 2334201 +quit

# make the target directory, if not exists
echo "Creating $targetDir..."
mkdir -p $targetDir

# copy the files from the depot to the target directory
echo "Copying files to $targetDir..."
cp -rv $HOME/.local/share/Steam/steamcmd/linux32/steamapps/content/app_2334200/depot_2334201/* $targetDir
```

You must change `$targetDir` to point at where you want the server to live.

Run this script, and you should have a folder in the place specified as `$targetDir`.

### Starting the server

Create a shell script called `theFront.sh` with these contents.

```bash
#! /usr/bin/bash

export USERDIR=$HOME/TheFrontServerData
export SERVERDIR=$HOME/TheFrontServer
export SERVERNAME="Server Name goes here"
# can have multiple admins, separate them by ';'
# these are steamids
export ADMINIDS=7xxxxxxxxxxxxxxxx
export IPADDRESS=31.x.x.x
export SERVERPASSWORD="a secret password goes here"
export SERVERWIPEDATE=2023-10-20
                    
mkdir -p $USERDIR

# change to the server folder specifed above
cd $SERVERDIR

# start the server
./ProjectWar/Binaries/Linux/TheFrontServer \
    ProjectWar_Start?DedicatedServer?MaxPlayers=8? \
    -server \
    -game -QueueThreshold=8 \
    -ServerName="$SERVERNAME" \
    -ServerAdminAccounts=$AdminSteamID \
    -log -locallogtimes \
    -EnableParallelCharacterMovementTickFunction \
    -EnableParallelCharacterTickFunction \
    -UseDynamicPhysicsScene \
    -OutIPAddress=$IPADDRESS \
    -ServerID=ANY_IDEA \
    -port=5001 \
    -BeaconPort=5002 \
    -QueryPort=5003 \
    -Game.PhysicsVehicle=false \
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
    -UserDir=$USERDIR

```

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

When logging out, use the `Ctrl-B D` command rather than Ctrl-C, Ctrl-C will stop the server.
