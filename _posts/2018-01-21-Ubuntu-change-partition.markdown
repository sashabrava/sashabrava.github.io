---
layout: article
title:  "Changing partition on Ubuntu 17.10"
date:   2018-01-21 12:00:00
categories: Ubuntu
description: "When Ubuntu reaches a space limit, it may stop loading after reboot. It's relevant especially for Ubuntu virtual machines. This is a guide of how problem appears and how to fix it. "
---
1. Description of the problem.

   It is quite a popular tool to launch Ubuntu on virtual machine in order to test some features or new tools. I personally use VirtualBox for it and  [Ubuntu 17.10 x64 distribution](http://releases.ubuntu.com/17.10/){:target="_blank"}.

   ![Ubuntu Create VM]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-create-vm.png)

   But, that's a common mistake to make a small drive partition (like 15-20 GB). And, after a while, when you have a plenty of software installed there, you get a message "Low Disk Space"

   ![Ubuntu Low Disk Space]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-low-space.png)

   So, the typical step is to shut down virtual machine and change the virtual disk size via Virtual Media Manager.

   ![Ubuntu Virtual Media Manager]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-vmm.png)


   But it doesn't help on Ubuntu start, it freezes with a black screen. And on the screen you can see the line "Started User Manager for UID 121", which means that Ubuntu can't load the graphic interface.

   ![Ubuntu launch error]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-launch-error.png) 


2. Fixing the problem.
   
   Firstly, you need to press "ESC" on Virtual Machine start (which will take you to Ubuntu book menu), and load Ubuntu in recovery mode. 

   ![Ubuntu Recovery Menu]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-recovery.png) 

   The tool ```dpkg``` will show exactly, if the problem is a lack of free place and how much free space operating system needs just to load GUI.

   ![Ubuntu dpkg]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-dpkg.png)

   Further steps are briefly described [here](https://askubuntu.com/a/116367){:target="_blank"}. So, your Ubuntu can fix this problem by its own means, no LiveCD required. Just use the tool ```root``` from Recovery Menu list, and paste following commands there: 

   ```sudo fdisk /dev/sda``` -- to run necessary tool;

   ```p``` -- to view listed partitions;

   ```d``` -- to delete dev/sda1;

   ```n``` -- to create a new one with partition type ```p``` and without removing the signature; 

   ![Ubuntu fdisk n]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-fdisk-n.png)

   ```a``` -- to make partition bootable;

   ```w``` -- to write changes.

   ![Ubuntu fdisk w]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-fdisk-w.png)

   After that command I additionally ran ```sudo mount -o remount, rw /``` because my hard drive was mounted in read-only mode, and ```sudo partprobe``` -- cause simple reboot, as it was adviced on Stackoverflow, didn't take an effect.

   ```sudo reboot```; 
 
   ```sudo mount -o remount, rw /``` and ```sudo resize2fs /dev/sda1```  -- to make the filesystem to take all available space on the partition.

   ![Ubuntu resize2fs]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-resize2fs.png)

   After these steps Ubuntu loads successfully.

   ![Ubuntu start screen]({{ site.url }}/assets/img/2018-01-21-Ubuntu-change-partition/ubuntu-success.png)



