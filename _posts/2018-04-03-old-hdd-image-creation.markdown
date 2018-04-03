---
layout: article
title:  "Safe migration from Windows XP to Linux Lite"
date:   2018-04-03 18:20:00
categories: Linux Lite
description: "There is a plenty of old PC in the storerooms. However, such devices still may have a second life - although they aren't powerful enough to support modern Linux and Windows distributions."
---
There is a plenty of old PC in the storerooms. However, such devices still may have a second life - although they aren't powerful enough to support modern Linux and Windows distributions. However, there are lightweight Linux distributions, which work well on old machines and at the same time contain latest updates.

The safest way to migrate on Linux from Windows XP (which was installed on most of computers from 2000th) is to check the health of hard drive (it has no point to experiment with pre-failure HDD) and to create an image of it - in order to have a backup OS if the experiment fails.

Since old hard drives had much smaller capacity, it's possible to backup the whole HDD on the computer (by saving disk image via shared folder through network (via SMB protocol) or by direct connection hard drive to the powerful computer, which will contain the image, via SATA/IDE to USB adapter.

![SATA/IDE to USB adapter]({{ site.url }}/assets/img/2018-04-03-old-hdd-image-creation/IDE-to-USB.JPG)

Disk image can be created by Disk Utility from Linux distributions (for example Ubuntu).

![Create disk image]({{ site.url }}/assets/img/2018-04-03-old-hdd-image-creation/Create-disk-image.png)

Creating disk images also allows to install certain Linux distribution, create an image of it, and then to install another distributions - however, there will be an easy way to return back to successful install if another distibution is less powerful or isn't supported by hardware. 

![Restore disk image]({{ site.url }}/assets/img/2018-04-03-old-hdd-image-creation/Restore-disk-image.png)
