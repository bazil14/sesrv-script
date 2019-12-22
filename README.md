# sesrv-script
Bash script for running Space Engineers on a linux server

**Required packages**

-xvfb

-rsync

-tmux

-wine

-winetricks

-steamcmd

-postfix (optional for email notifications)

-zip (optional but required if using the email feature)

**Features:**

-auto backups

-auto updates

-script logging

-auto restart if crashed

-delete old backups

-delete old logs

-run from ramdisk

-sync from ramdisk to hdd/ssd

-start on os boot

-shutdown gracefully on os shutdown

-script auto update from github

-send email notifications after 3 crashes within a 5 minute time limit (optional)

-send email notifications when server updated (optional)

**Instructions:**

Log in to your server with ssh and execute:

```git clone https://github.com/7thCore/sesrv-script```

Make it executable:

```chmod +x ./sesrv-script.bash```

If you plan on using a ramdisk to run your server from, the script will give you that option.

Now for the installation.

If you wish you can have the script install the required packages with (Only for Arch Linux & Ubuntu 19.10):

```sudo ./sesrv-script.bash -install_packages```

After that run the script with root permitions like so (necessary for user creation):

```sudo ./sesrv-script.bash -install```

The script will create a new non-sudo enabled user from wich the game server will run. If you want to have multiple game servers on the same machine just run the script multiple times but with a diffrent username inputted to the script.

After the installation finishes you can reboot the operating system and the service files will start the game server automaticly on boot.

You can also install bash aliases to make your life easier with the following command:

```./sesrv-script.bash -install_aliases```

After that relog.

Any other script commands are available with:

```./sesrv-script.bash -help```

**World and config files**

The easiest way to do this is to just generate them locally and copy them over to the server, this can be done by using the dedicated server tool on your windows box, the tool is located in

```[Steam intall directory]\steamapps\common\SpaceEngineers\Tools\DedicatedServer\SpaceEngineersDedicated.exe```

    Select the Default profile
    Set up the world
    Save the config
    Start to generate the world.

The files will be stored in

```C:\Users\{USERNAME}\AppData\Roaming\SpaceEngineersDedicated\Default```

Edit the ```SpaceEngineers-Dedicated.cfg``` and copy it with the ```Saves``` folder to the following directory on your Linux box

```/home/$(whoami)/server/drive_c/users/$(whoami)/Application\ Data/SpaceEngineersDedicated```

**Tweaks**

You'll have to change the <LoadWorld> tag so it point to the correct directory.

If the Save folder is located in

```/home/$(whoami)/server/drive_c/users/$(whoami)/Application\ Data/SpaceEngineersDedicated/Saves/Created 2015-03-30 2331```

the ```<LoadWorld>``` tag must look like this, where ```{username}``` is the same as ```$(whoami)```

```<LoadWorld>C:\Users\{username}\Application Data\SpaceEngineersDedicated\Default\Saves\Created 2015-03-30 2331</LoadWorld>``` 

You still need to use windows paths.

That should be it.

**Known issues are:**

-Wine version 4.12 and later are fubar. Use 4.11 or lower.

-The winetricks package in ubuntu is outdated. Follow this guide to install the latest winetricks: https://wiki.winehq.org/Winetricks (needed for dotnet472)

-if for some reason systemd reports the service failed when it stops, don't worry about it, the server session shuts down gracefully. (This should be solved)

**Disclamer**

Credit goes to github users CoffeePirate and tvwenger. I stumbled upon  CoffeePirate's github page and tvwenger's comment on the same page on how to get it to work. I just modified one of my existing scripts so it would be a bit easier to install and manage the server.

The page is located here:
https://gist.github.com/CoffeePirate/102e789310719cad6457
