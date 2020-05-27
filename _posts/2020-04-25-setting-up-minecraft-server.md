---
layout: post
title:  "Setting up your own Minecraft server"
date:   2020-04-25 10:36:00 +0200
categories: programming
tags: [servers, minecraft]
---


The kids want to play Minecraft with friends and you want a safe sandbox. There are a lot of tutorials on the internet but none of them answer all necessary questions ([This one](https://linuxize.com/post/how-to-install-minecraft-server-on-ubuntu-18-04/) was the best, though). Based on that, and some additional bits here and there, this is how I set up a server.

Ingredients needed:

-  Players should have Minecraft Java versions. I don't understand why it wouldn't work for other Minecraft versions but could not test this.
-  Linux server. I have a Ubuntu 18.04 server running anyway so I used that.
-  Internet router where you can set port forwarding.
-  (Optionally) dynamic DNS service. This makes accessing the server nicer. I am using [DuckDNS](https://www.duckdns.org/)

## On the Linux server

Install the headless Java Runtime needed by Minecraft server and wget if you somehow don't have it already:
```
sudo apt install openjdk-8-jre-headless wget
```

Create separate user for Minecraft:
```
sudo useradd -r -U -m -d /opt/minecraft -s /bin/bash minecraft
```

(Meaning of options: -r for creating minecraft-user as system user, -U to create a group for this user, -m to create a home directory and -d to pass directory location as parameter.)

Switch to minecraft user role:
```
sudo su - minecraft
```

Create a directory for Minecraft server files, enter the directory and download the most recent [Minecraft server file](https://www.minecraft.net/download/server/):
```
mkdir ~/server && cd ~/server
wget https://launcher.mojang.com/v1/objects/bb2b6b1aefcd70dfd1892149ac3a215f6c636b07/server.jar
```

Start the server (this will fail but is necessary):
```
java -Xmx1024M -Xms512M -jar server.jar nogui
```

You get an error message saying you haven't accepted license agreement. To accept it, edit *eula.txt* and set eula key from false to true:
```
eula=true
```

Also (this is what I didn't do first time around), configure the server to your liking. For example, to set the gamemode to creative (default is survival), enable console, and set up a whitelist for allowed players (else, anybody who knows the address can join), change these:
```
force-gamemode=true
...
gamemode=creative
...
enable-command-block=true
...
white-list=true
```

If you use whitelist, there needs to be a *whitelist.json* file in the server directory with the format:
```
[
  {
    "uuid": "f430dbb6-5d9a-444e-b542-e47329b2c5a0",
    "name": "username"
  },
  {
    "uuid": "e5aa0f99-2727-4a11-981f-dded8b1cd032",
    "name": "username"
  }
]
```

Usernames are the usernames of the players. uuid's can be obtained using [a service for that](https://mcuuid.net/).

Log out the minecraft user:
```
exit
```

Create a system service to start the server automatically:
```
sudo nano /etc/systemd/system/minecraft.service
```

Give service this configuration:
```
[Unit]
Description=Minecraft Server
After=network.target

[Service]
User=minecraft
Nice=1
KillMode=none
SuccessExitStatus=0 1
ProtectHome=true
ProtectSystem=full
PrivateDevices=true
NoNewPrivileges=true
WorkingDirectory=/opt/minecraft/server
ExecStart=/usr/bin/java -Xmx1024M -Xms512M -jar server.jar nogui

[Install]
WantedBy=multi-user.target
```

Start and enable the service:
```
sudo systemctl daemon-reload
sudo systemctl start minecraft
sudo systemctl enable minecraft
```

(Optionally) adjust firewall settings, if you are using uwf firewall. 25565 is the default Minecraft server port:
```
sudo ufw allow 25565/tcp
```

Then login to your internet router and set up single port forwarding for outside port 25565 to inside port 25565 (only TCP needed).

Now, if you don't have a Dynamic dns, figure out your outside IP address using for example [whatismyip](https://www.whatismyip.com/).

Then start Minecraft on clients and start multiplayer givin that IP address, or the dynamic DNS address you have configured. If you are using the default port 25565, you don't need port information. Works like a charm.

