# sesrv-script
Bash script for running Space Engineers on a linux server

-------------------------

# What does this script do?

This script creates a new non-sudo enabled user and installes the game in a folder called server in the user's home folder. It also installes systemd services for starting and shutting down the game server when the computer starts up, shuts down or reboots and also installs systemd timers so the script is executed on timed intervals (every 15 minutes) to do it's work like automatic game updates, backups and syncing from ramdisk to hdd. It will also create a config file in the script folder that will save the configuration you defined between the installation process. The reason for user creation is to limit the script's privliges so it CAN NOT be used with sudo when handeling the game server. Sudo is only needed for installing the script (for user creation) and installing packages (it the script supports the distro you are running).

-------------------------

**Features:**

- auto backups

- auto updates

- script logging

- auto restart if crashed

- delete old backups

- delete old logs

- run from ramdisk

- sync from ramdisk to hdd/ssd

- start on os boot

- shutdown gracefully on os shutdown

- script auto update from github

- send email notifications after 3 crashes within a 5 minute time limit (optional)

- send email notifications when server updated (optional)

- send email notifications on server startup (optional)

- send email notifications on server shutdown (optional)

- send discord notifications after 3 crashes within a 5 minute time limit (optional)

- send discord notifications when server updated (optional)

- send discord notifications on server startup (optional)

- send discord notifications on server shutdown (optional)

- supports multiple discord webhooks

-------------------------

# Supported distros

- Arch Linux

- Ubuntu 19.10

I will add suport for the next Ubuntu LTS version when it's released.

The script can, in theory run on any systemd-enabled distro. So if you are not using Arch Linux or Ubuntu 19.10 I suggest you check your distro's wiki on how to install the required packages. The script can, in theory install packages for any Ubuntu version, but the repositories for old versions of Ubuntu might have outdated packages and cause problems.

-------------------------

# WARNING

- Script updates from GitHub: These may include malicious code to steal any info the script uses to work, like email credentials and discord webhooks.
Now I'm not saying that I'm that kind of person that would do that but:

**IF YOU DON'T TRUST ME, LEAVE THIS OFF FOR SECURITY REASONS!**

-------------------------

# Installation

**Required packages**

- xvfb

- rsync

- tmux (minimum version: 2.9a)

- wine

- winetricks

- steamcmd

- curl

- wget

- cabextract

- postfix (optional for email notifications)

- zip (optional but required if using the email feature)

-------------------------

**Download the script:**

Log in to your server with ssh and execute:

`git clone https://github.com/7thCore/sesrv-script`

Make it executable:

`chmod +x ./sesrv-script.bash`

-------------------------

**Installation:**

If you wish you can have the script install the required packages with (Only for Arch Linux & Ubuntu 19.10):

`sudo ./sesrv-script.bash -install_packages`

After that run the script with root permitions like so (necessary for user creation):

`sudo ./sesrv-script.bash -install`

You can also install bash aliases to make your life easier by logging in to the newly created user and executing the script with the following command:

`./sesrv-script.bash -install_aliases`

After the installation finishes you can log in to the newly created user and fine tune your game configuration and then reboot the operating system and the service files will start the game server automaticly on boot.

-------------------------

**World and config files**

The easiest way to do this is to just generate them locally and copy them over to the server, this can be done by using the dedicated server tool on your windows box, the tool is located in

`[Steam intall directory]\steamapps\common\SpaceEngineers\Tools\DedicatedServer\SpaceEngineersDedicated.exe`

    Select the Default profile
    Set up the world
    Save the config
    Start to generate the world.

The files will be stored in

`C:\Users\{USERNAME}\AppData\Roaming\SpaceEngineersDedicated\Default`

Edit the `SpaceEngineers-Dedicated.cfg` and copy it with the `Saves` folder to the following directory on your Linux box

`/home/$(whoami)/server/drive_c/users/$(whoami)/Application\ Data/SpaceEngineersDedicated`

**Tweaks**

You'll have to change the <LoadWorld> tag in SpaceEngineers-Dedicated.cfg so it point to the correct directory.

If the Save folder is located in

`/home/space_engineers/server/drive_c/users/space_engineers/Application\ Data/SpaceEngineersDedicated/Saves/Created 2015-03-30 2331`

the `<LoadWorld>` tag must look like this, where `{username}` is the same as `$(whoami)`

`<LoadWorld>C:\Users\space_engineers\Application Data\SpaceEngineersDedicated\Default\Saves\Created 2015-03-30 2331</LoadWorld>` 

You still need to use windows paths.

That should be it.

-------------------------

# Available commands:

| Command | Description |
| ------- | ----------- |
| `-help` | Prints a list of commands and their description |
| `-start` | Start the server |
| `-stop` | Stop the server |
| `-restart` | Restart the server |
| `-sync` | Sync from tmpfs to hdd/ssd |
| `-backup` | Backup files, if server running or not. |
| `-autobackup` | Automaticly backup files when server running |
| `-deloldbackup` | Delete old backups |
| `-delete_save` | Delete the server's save game with the option for deleting/keeping the SpaceEngineers-Dedicated.cfg file |
| `-deloldsavefiles` | Delete the server's save game (leaves the latest number files specefied in the script conf file |
| `-change_branch` | Changes the game branch in use by the server (public,experimental,legacy and so on) |
| `-install_aliases` | Installs .bashrc aliases for easy access to the server tmux session |
| `-rebuild_tmux_config` | Reinstalls the tmux configuration file from the script. Usefull if any tmux configuration updates occoured |
| `-rebuild_services` | Reinstalls the systemd services from the script. Usefull if any service updates occoured |
| `-rebuild_prefix` | Reinstalls the wine prefix. Usefull if any wine prefix updates occoured |
| `-disable_services` | Disables all services. The server and the script will not start up on boot anymore |
| `-enable_services` | Enables all services dependant on the configuration file of the script |
| `-reload_services` | Reloads all services, dependant on the configuration file |
| `-update` | Update the server, if the server is running it wil save it, shut it down, update it and restart it |
| `-update_script` | Check github for script updates and update if newer version available |
| `-update_script_force` | Get latest script from github and install it no matter what the version |
| `-status` | Display status of server |
| `-install` | Installs all the needed files for the script to run, the wine prefix and the game |
| `-install_packages` | Installs all the needed packages (Supports only Arch linux & Ubuntu 19.10 and onward)

-------------------------

**Known issues are:**

-The winetricks package in ubuntu is outdated. Follow this guide to install the latest winetricks: https://wiki.winehq.org/Winetricks (needed for dotnet472)

-------------------------

**Disclamer**

Credit goes to github users CoffeePirate and tvwenger. I stumbled upon  CoffeePirate's github page and tvwenger's comment on the same page on how to get it to work. I just modified one of my existing scripts so it would be a bit easier to install and manage the server.

The page is located here:
https://gist.github.com/CoffeePirate/102e789310719cad6457
