---
layout: post
title:  "Configuring hibernate on lid close in Ubuntu"
date:   2020-11-22 12:04:00 +0200
categories: linux
tags: [ubuntu, linux, configuration]
---

Ubuntu 20.04 that I am using on my Thinkpad T460S often runs out of battery when I am not using the laptop for some time and I have lid closed. Being a long time Macbook user I know that power-saving can work fantastically on a laptop but the problem was never so big I took time to investigate. Well, now I decided to figure out if same convenience can be achieved on Linux. It can and it is not hard. Things are just not configured to my liking by default.

Two key configuration files are /etc/systemd/logind.conf and /etc/systemd/sleep.conf and they have very well written man pages. The first one has options to set what happens when lid is closes. 

``` 
# /etc/systemd/logind.conf

[Login]
HandleLidSwitch=suspend-then-hibernate
HandleLidSwitchExternalPower=suspend-then-hibernate
```

The default value for HandleLidSwitch is "suspend" which slowly drains the battery and depletes it completely in 1-3 days even from full charge. To set the computer to first suspend to RAM and then hibernate after a time interval "suspend-then-hibernate" should be used. 

However, the **real gotcha** here is that one should also set HandleLidSwitchExternalPower to same value, or to any value, even just "suspend"! According to man page, this option is completely ignored (for backwards compatibility) by default. So when you close the lid with power cord on, no power saving is triggerd. Then when you decide to unplug the power cord to put the laptop in your bag for next day or something, you are still not in power-save mode of any kind you'll find that the battery is completely depleted when you need it. (I don't have a Dock but there is also option to set the value for lid close behaviour when docked.)

The default delay for "suspend-then-hibernate" seems to be 180 minutes. For my purposes, two hours is more appropriate so to configure that: 

```
# /etc/systemd/sleep.conf

[Sleep]
HibernateDelaySec=120min
```

So with these options, whenever I close my laptop lid, the system suspends to RAM for two hours. This means that if I go out for lunch, take a short commute or have other kind of short break, the system wakes up very fast when I open the lid. But after two hours, the system hibernates to save battery. My system wakes up from hibernate to existing state in 15-30 seconds depending on scenario and that is quite acceptable.
