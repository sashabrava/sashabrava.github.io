---
layout: article
title:  "Recover data from HDD"
date:   2018-08-30 21:52:00 +0300
categories: HDD
description: "HDD can stop spinning at the most unexpected moment, resulting data loss. However, there are ways to avoid it."
---
Sometimes it happens, that HDD, containing important data, stops working. Everybody knows about necessity of making backups, but for various reasons people often don't make them... Therefore it's quite a common story, when files need to be recovered from broken hard drive.

In my case the hard drive wasn't spinning at all (although the HDD motor tried to spin on startup). It gave rise to suspicion, that HDD head stuck on the platter. After the hard drive was opened up, the suspicion appeared to be true.

![Opened HDD]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/opened.png) 

Sometimes the head gets damaged after getting stuck on the platter, and there is no way to read data except to replace the head. Hopefully, in my case it was enough to park the head on its place, and the HDD started to spin up and read data.

![HDD head parked]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/head-parked.png)
 
After the hard drive started spinning, OS Linux detected it - but the attempt to mount data partition resulted with an error.

![Mount error]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/mount-error.png)

The first thing to do was to make a snapshot of broken partition in order to get as max data as possible. I used a `ddrescue` command for that. There are ddrescue programs of different authors, and the difference is described <a target="_blank" href="https://askubuntu.com/a/211579">here</a>.


At first, I just read the partition without stopping on errors. Full description of used options can be found <a target="_blank" href="https://www.gnu.org/software/ddrescue/manual/ddrescue_manual.html" >here</a>.

![First ddrescue execution]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/ddrescue-fast.png)

There are various ways to read broken sectors. I turned on "direct disk access" (`-d` option) and removed options of skipping error blocks.

![Second ddrescue execution]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/ddrescue-scrap.png)

It resulted with reading most of HDD partition, with a small data loss.
As a mean of recovering data, I may have used `testdisk`. But it has one big disadvantage - it can't restore folder structure, which was a key necessity for me (since NTFS partition table wasn't completely lost). That's why I used R-STUDIO for my purpose.

![R-Studio]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/r-studio.png)

R-STUDIO scanned an image of hard drive and found several partitions there. Since I knew that partition had NTFS filesystem, I chose the 1st one for data recovery.

![R-Studio recover]({{ site.url }}/assets/img/2018-08-30-recover-data-from-hdd/r-studio2.png)

Data recovery was completed successfully.


