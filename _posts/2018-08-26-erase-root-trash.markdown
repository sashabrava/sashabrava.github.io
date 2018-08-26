---
layout: article
title:  "Erasing root Trash on Ubuntu 18.10"
date:   2018-08-26 12:52:00
categories: Ubuntu
description: "Ubuntu Desktop users can delete files using Nautilus, but it causes free space problems"
---
Ubuntu Desktop users can delete files from root user (by Nautilus app launched as `sudo nautilus /`), and it causes free space problems. From the Nautilus settings, root user doesn't have a Trash. Although, if you delete files from root the usual way (by pushing `Del` button for example), they are placed into the trash folder. But they aren't visible in the trash of user from GUI.

In my case running out of free space ended up with system not booting on startup and me using Recovery mode to find the problem.

First thing to do in Recovery mode is launching `df` command to check free space available on hard drive. In my case it was 0  bytes.

![Command df]({{ site.url }}/assets/img/2018-08-26-erase-root-trash/df.png)

There is a trick - Ubuntu reserves 5% of the hard drive for system services. If the hard drive is big, you can set this parameter to 0%, and the system will get a necessary amount of space to launch. You can read more about it <a target="_blank" href="https://odzangba.wordpress.com/2010/02/20/how-to-free-reserved-space-on-ext4-partitions/">here</a>.

That's how changing this parameter looks like (`/dev/sda1` used space decreased from 24% to 23% on screenshot below) after running the command `sudo tune2fs -m 0 /dev/sda1` (don't forget to run `sudo tune2fs -m 5 /dev/sda1` after you fix a free space issue).

![Command tune2fs]({{ site.url }}/assets/img/2018-08-26-erase-root-trash/tune2fs.png)

After I launched the system in a usual mode, I used `sudo du -hs /home` command to check my users folder size.

![Command du]({{ site.url }}/assets/img/2018-08-26-erase-root-trash/du.png)

Since I didn't have big files stored there, the size was too big. Although the trash looked empty from GUI.

![Empty trash]({{ site.url }}/assets/img/2018-08-26-erase-root-trash/trash.png)

 However, I found deleted from root files in Trash folder (full path to files is `/home/user/.local/share/Trash/files`) and removed them using `rm` command - and, when I looked at folder size again, it looked like that.

![Command du]({{ site.url }}/assets/img/2018-08-26-erase-root-trash/du2.png)

A couple of experiments showed, that users must be careful while using Ubuntu Disk application - because it showes that you have free space even if you system is out of it and there are only 5% reserved for system left.

![Disks application]({{ site.url }}/assets/img/2018-08-26-erase-root-trash/disks.png)


