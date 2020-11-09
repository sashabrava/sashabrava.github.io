---
layout: article
title:  "Deeper research of broken HDD"
date:  2020-11-09 21:18:00 +0100
categories: [HDD, MHDD, FreeDOS]
description: "Guide how to make a research of falling HDD from FreeDOS"
---

### Introduction

In the <a target="_blank" href="/2020/recovering-windows-data">previous article</a>, I got an access to data on failing HDD and even temporarily fixed NTFS table. This HDD was gifted for tech experiments, and it was a nice occasion to make a deeper diagnostics of the device. Drive SMART before the research looked like this.

![SMART before research]({{ site.url }}/assets/img/2020-10-20-hdd/hdd-during.png)

### Preparing tools

I was surprised to notice, that there aren't many tools for HDD testing, especially for Linux. Some of them (Victoria or MHDD) are based on MS-DOS and were written over 15 years ago, but still usable. In order to use <a target="_blank" href="https://hddguru.com/software/2005.10.02-MHDD/">MHDD</a>, PC should support running hard drives (even SATA ones) in IDE legacy mode. As a testing platform, MS-DOS is recommended by the software author. However, I managed to run it from 
<a target="_blank" href="https://www.freedos.org/download/">FreeDOS</a>, an open-source DOS-compatible environment. 

In order to create bootable pen drive, I used the command `sudo dd if=PATH_TO_FD12LITE.img of=/dev/sda status=progress && sync`. The first partition is a bootable one from the image, and the second partition was created manually to copy MHDD files and to store its logs. Since the program was written many years ago, last release dates back to 2005, I made a small FAT32 partition for data storage.
 

![Bootable pen drive]({{ site.url }}/assets/img/2020-11-09-hdd-research/partitioning-mhdd-usb.png)


### Starting the research

After the bootable device is created and FreeDOS is launched, your should choose the language and select option to run without installation "No, return to DOS".

![FreeDOS]({{ site.url }}/assets/img/2020-11-09-hdd-research/freedos.png)

The interface of MHDD is quite scary for new users and looks like this. However, there is an <a target="_blank" href="https://hddguru.com/software/2005.10.02-MHDD/mhdd_manual.en.html">instruction</a>  how to use it.

![MHDD Interface]({{ site.url }}/assets/img/2020-11-09-hdd-research/mhdd-interface.jpg)

Step numer one is connecting HDD to MHDD. First your should type `Shift + F3` to select HDD for scanning.

![MHDD choose drive]({{ site.url }}/assets/img/2020-11-09-hdd-research/mhdd-choose-drive.jpg)

Another step was simple scan of HDD surface to check for bad blocks, without parameters. The result of `SCAN` command is visible on HDD screen and is written to LOG/MHDD.LOG file.


```
6.03.2012  19:15:47   MHDD>SCAN 
 6.03.2012  19:15:53   Scan started
 6.03.2012  19:15:53   MODE: IDE
 6.03.2012  19:15:53   Device: ST1000LM024 HN-M101MBB
 6.03.2012  19:15:53   -------------------------------
 6.03.2012  19:15:53   Lap : 1
 6.03.2012  19:15:53   LBA scan: 0 to 1953525167
 6.03.2012  19:18:46   þ LBA warning: 37671660
 6.03.2012  19:19:16   þ LBA warning: 44103015
 6.03.2012  19:19:16   þ LBA Warning: 44103270
 6.03.2012  19:19:38   þ LBA Error: 48350664
 6.03.2012  19:19:41   þ LBA Error: 48482768
...
 6.03.2012  22:47:42   Time spent: 03:31:46
 6.03.2012  22:47:42    Blocks <   3ms = 7560949
 6.03.2012  22:47:42    Blocks <  10ms = 749635
 6.03.2012  22:47:42    Blocks <  50ms = 1382
 6.03.2012  22:47:42    Blocks < 150ms = 36
 6.03.2012  22:47:42    Blocks < 500ms = 19
 6.03.2012  22:47:42    Blocks > 500ms = 6
 6.03.2012  22:47:42   Errors: 32, Warnings: 25
 6.03.2012  22:47:42   Done
```
 
After the scan I fully erased the HDD via command `ERASE`. It was necessary since sometimes HDD generates soft bad blocks, which were caused by OS errors, but not hardware fault of those sectors. It wasn't quite my story (since tested HDD had 0 bad blocks, but continuos read errors), still it was worth trying.


SMART (`F8` shortcut) before HDD erase.

```
6.03.2012  19:04:12   MHDD>SMART ATT
 6.03.2012  19:04:12   HDD: ST1000LM024 HN-M101MBB
 6.03.2012  19:04:12   --------------------------------------------------------
 6.03.2012  19:04:12   SMART attributes:
 6.03.2012  19:04:12               Name                        Val Worst Raw
 6.03.2012  19:04:12   Att #   1 : Read error rate           : 100  100  4927  
 6.03.2012  19:04:12   Att #   5 : Reallocated sectors count : 252  252  0  
  6.03.2012  19:04:12   Att #   9 : Power-on time             : 100  100  10604  
  6.03.2012  19:04:12   Att # 194 : HDA Temperature           :  60   46  40  
 6.03.2012  19:04:12   Att # 195 : Hardware ECC recovered    : 100  100  0  
 6.03.2012  19:04:12   Att # 196 : Reallocate event count    : 252  252  0  
 6.03.2012  19:04:12   Att # 197 : Current pending sectors   :  99   99  230  
 6.03.2012  19:04:36   Att # 200 : Write error rate          : 100  100  25719  
```


```
 6.03.2012  19:06:27   MHDD>ERASE 
 6.03.2012  19:06:28   ST1000LM024 HN-M101MBB  LBA: 1,953,525,168

```

SMART `#200 Write error rate` didn't change after the `erase`, as well as other parameters, and I started `scan` procedure with `erase` option. This option tries to fill in with zeros those blocks, which are unreadable.
```
6.03.2012  19:09:37   MHDD>SCAN 
 6.03.2012  19:09:40   Scan started
 6.03.2012  19:09:40   MODE: IDE
 6.03.2012  19:09:40   ERASE DELAYS: ON, TIME=350 msec
 6.03.2012  19:09:40   Device: ST1000LM024 HN-M101MBB
 6.03.2012  19:09:40   -------------------------------
 6.03.2012  19:09:40   Lap : 1
 6.03.2012  19:09:40   LBA scan: 0 to 1000000000
 6.03.2012  19:12:07   Erase 255 sectors starting from 32136885
 6.03.2012  19:12:11   Erase 255 sectors starting from 32136885
 6.03.2012  19:12:14   þ LBA Error: 32137140
...
 6.03.2012  19:16:29   Erase 255 sectors starting from 32152185
 6.03.2012  19:16:29   þ LBA Timeout: 32152185
 6.03.2012  19:16:29   Last scanned LBA: 32152439
 6.03.2012  19:16:29   Blocks erased with EraseWaits: 72
 6.03.2012  19:16:29    Blocks <   3ms = 125868
 6.03.2012  19:16:29    Blocks <  10ms = 161
 6.03.2012  19:16:29    Blocks <  50ms = 5
 6.03.2012  19:16:29    Blocks < 150ms = 2
 6.03.2012  19:16:29    Blocks < 500ms = 5
 6.03.2012  19:16:29    Blocks > 500ms = 0
 6.03.2012  19:16:29   Errors: 11, Warnings: 5
 6.03.2012  19:16:29   Done
 6.03.2012  19:16:32   ST1000LM024 HN-M101MBB  LBA: 1,953,525,168
```

In a while I continued the scan and the result looked like this. 

```
6.03.2012  19:16:54   MHDD>SCAN 
 6.03.2012  19:17:06   Scan started
 6.03.2012  19:17:06   MODE: IDE
 6.03.2012  19:17:06   ERASE DELAYS: ON, TIME=350 msec
 6.03.2012  19:17:06   Device: ST1000LM024 HN-M101MBB
 6.03.2012  19:17:06   -------------------------------
 6.03.2012  19:17:06   Lap : 1
 6.03.2012  19:17:06   LBA scan: 32146065 to 1000000000
 6.03.2012  19:17:08   Erase 255 sectors starting from 32146065
 6.03.2012  19:17:20   Erase 255 sectors starting from 32146065
 6.03.2012  19:17:20   þ LBA Timeout: 32146065
 6.03.2012  19:17:20   þ LBA Warning: 32146320
 6.03.2012  19:17:21   þ LBA Error: 32147850
 6.03.2012  19:17:21   þ LBA Warning: 32148105
 6.03.2012  19:17:21   Erase 255 sectors starting from 32148360
 6.03.2012  19:17:25   Erase 255 sectors starting from 32148360
 6.03.2012  19:17:25   þ LBA Warning: 32148615
 6.03.2012  19:17:26   Erase 255 sectors starting from 32152440
 6.03.2012  19:17:34   Erase 255 sectors starting from 32152440
 6.03.2012  19:17:36   þ LBA Warning: 32152695
 6.03.2012  19:17:37   þ LBA Warning: 32154735
 6.03.2012  19:25:59   Erase 255 sectors starting from 140557530
 6.03.2012  19:26:04   Erase 255 sectors starting from 140557530
 6.03.2012  19:26:07   þ LBA Error: 140557785
 6.03.2012  19:26:07   Erase 255 sectors starting from 140558040
 6.03.2012  19:26:11   Erase 255 sectors starting from 140558040
...
 6.03.2012  19:31:32   Erase 255 sectors starting from 182151345
 6.03.2012  19:31:33   þ LBA Warning: 182152875
 6.03.2012  19:31:34   Erase 255 sectors starting from 182153640
 6.03.2012  19:31:37   Erase 255 sectors starting from 182153640
 6.03.2012  20:41:10   Blocks erased with EraseWaits: 44
 6.03.2012  20:41:12   Time spent: 01:24:03
 6.03.2012  20:41:12    Blocks <   3ms = 3566480
 6.03.2012  20:41:12    Blocks <  10ms = 228878
 6.03.2012  20:41:12    Blocks <  50ms = 103
 6.03.2012  20:41:12    Blocks < 150ms = 10
 6.03.2012  20:41:12    Blocks < 500ms = 8
 6.03.2012  20:41:12    Blocks > 500ms = 0
 6.03.2012  20:41:12   Errors: 5, Warnings: 8
 6.03.2012  20:41:12   Done
```

I have noticed a weird thing, sector addresses of bad blocks were absolutely different from bad blocks at the first scan. For example there haven't been any problems with reading blocks after 100,000,000. Overall amount of errors was smaller than during first scan. Since I knew the position of badly readable blocks, I decided to try remapping them.

```
6.03.2012  20:52:38   MHDD>SCAN 
 6.03.2012  20:53:14   Scan started
 6.03.2012  20:53:14   MODE: IDE
 6.03.2012  20:53:14   REMAP: ON
 6.03.2012  20:53:14   Device: ST1000LM024 HN-M101MBB
 6.03.2012  20:53:14   -------------------------------
 6.03.2012  20:53:14   Lap : 1
 6.03.2012  20:53:14   LBA scan: 0 to 182146760
 6.03.2012  20:55:40   þ LBA Error: 32137140
 6.03.2012  20:59:01   À> Remap try...
 6.03.2012  20:59:01   þ LBA Error: 32137141
 6.03.2012  21:02:22   À> Remap try...
 6.03.2012  21:02:22   þ LBA Error: 32137142
 6.03.2012  21:05:42   À> Remap try...
...
 6.03.2012  22:16:09   À> Remap try...
 6.03.2012  22:16:09   þ LBA Error: 32137164
 6.03.2012  22:19:29   À> Remap try...
 6.03.2012  22:19:30   þ LBA Timeout: 32137165
 6.03.2012  22:19:30   Last scanned LBA: 32137419
 6.03.2012  22:19:30    Blocks <   3ms = 125867
 6.03.2012  22:19:30    Blocks <  10ms = 147
 6.03.2012  22:19:30    Blocks <  50ms = 13
 6.03.2012  22:19:30    Blocks < 150ms = 1
 6.03.2012  22:19:30    Blocks < 500ms = 0
 6.03.2012  22:19:30    Blocks > 500ms = 0
 6.03.2012  22:19:30   Errors: 25, Warnings: 0
 6.03.2012  22:19:30   Done
```

Seems that HDD had a huge problem with remapping bad blocks, which is a scary sign. However, HDD was doing well with writing data to there blocks, since `ERASE` command  didn't cause any new SMART errors. 


SMART showed following parameters:

```
6.03.2012  23:26:35   MHDD>SMART ATT
 6.03.2012  23:26:35   Getting SMART attributes...
 6.03.2012  23:26:35   SMART READ ATTRIBUTES
 6.03.2012  23:26:35   HDD: ST1000LM024 HN-M101MBB
 6.03.2012  23:26:35   --------------------------------------------------------
 6.03.2012  23:26:35   SMART attributes:
 6.03.2012  23:26:35               Name                        Val Worst Raw
 6.03.2012  23:26:35   Att #   1 : Read error rate           : 100  100  5270  
 6.03.2012  23:26:35   Att #   5 : Reallocated sectors count : 252  252  0  
 6.03.2012  23:26:35   Att #   9 : Power-on time             : 100  100  10615  
 6.03.2012  23:26:35   Att # 194 : HDA Temperature           :  56   45  44  
 6.03.2012  23:26:35   Att # 196 : Reallocate event count    : 252  252  0  
 6.03.2012  23:26:35   Att # 197 : Current pending sectors   : 100   99  24  
 6.03.2012  23:27:15   Att # 200 : Write error rate          : 100  100  25719  
```

The amount of pending sectors (ones, that could not be read) has decreased, but still there weren't any bad ones.

Hard disk was turned off, and in a while `SCAN` option was launched again. Result was just unpredictable, 0 read errors.

```
 6.03.2012  20:08:20   MHDD>SCAN 
 6.03.2012  20:08:41   Scan started
 6.03.2012  20:08:41   MODE: IDE
 6.03.2012  20:08:41   ERASE DELAYS: ON, TIME=350 msec
 6.03.2012  20:08:41   Device: ST1000LM024 HN-M101MBB
 6.03.2012  20:08:41   -------------------------------
 6.03.2012  20:08:41   Lap : 1
 6.03.2012  20:08:41   LBA scan: 0 to 1000000000
 6.03.2012  21:32:18   Blocks erased with EraseWaits: 0
 6.03.2012  21:32:19   Time spent: 01:23:36
 6.03.2012  21:32:19    Blocks <   3ms = 3692574
 6.03.2012  21:32:19    Blocks <  10ms = 228916
 6.03.2012  21:32:19    Blocks <  50ms = 74
 6.03.2012  21:32:19    Blocks < 150ms = 5
 6.03.2012  21:32:19    Blocks < 500ms = 0
 6.03.2012  21:32:19    Blocks > 500ms = 0
 6.03.2012  21:32:19   No warnings, no errors
 6.03.2012  21:32:19   Done
```

And SMART still showed zero reallocated rectors.

```
6.03.2012  21:38:19   MHDD>SMART ATT
 6.03.2012  21:38:19   Getting SMART attributes...
 6.03.2012  21:38:19   SMART READ ATTRIBUTES
 6.03.2012  21:38:19   HDD: ST1000LM024 HN-M101MBB
 6.03.2012  21:38:19   --------------------------------------------------------
 6.03.2012  21:38:19   SMART attributes:
 6.03.2012  21:38:19               Name                        Val Worst Raw
 6.03.2012  21:38:19   Att #   1 : Read error rate           : 100  100  5345  
 6.03.2012  21:38:19   Att #   5 : Reallocated sectors count : 252  252  0  
 6.03.2012  21:38:19   Att #   9 : Power-on time             : 100  100  10625  
 6.03.2012  21:38:19   Att # 194 : HDA Temperature           :  43   43  57  
 6.03.2012  21:38:19   Att # 196 : Reallocate event count    : 252  252  0  
 6.03.2012  21:38:19   Att # 197 : Current pending sectors   : 252   99  0  
 6.03.2012  21:38:28   Att # 200 : Write error rate          : 100  100  25719  
```

As a matter of experiment, I installed clean Debian-based system on this HDD, and in a while I could see errors during OS boot and an increased number of errors in SMART.


```
 6.03.2012  19:11:15   MHDD>SMART ATT
 6.03.2012  19:11:15   Getting SMART attributes...
 6.03.2012  19:11:15   SMART READ ATTRIBUTES
 6.03.2012  19:11:15   HDD: ST1000LM024 HN-M101MBB
 6.03.2012  19:11:15   --------------------------------------------------------
 6.03.2012  19:11:15   SMART attributes:
 6.03.2012  19:11:15               Name                        Val Worst Raw
 6.03.2012  19:11:15   Att #   1 : Read error rate           : 100  100  5349  
 6.03.2012  19:11:15   Att #   9 : Power-on time             : 100  100  10635  
 6.03.2012  19:11:19   Att # 200 : Write error rate          : 100  100  25756  
```

### Conclusion of the research

1. This HDD seem to have a problem with reading blocks at different sectors, which looks like problem with read/write heads.
2. Hard disks can get data from read-error sectors after device reboot or during next scans, which means that full disk image can be created via `dd_rescue` via multiple disk reads even without a costly replacement of read-write head from another HDD of that exact model.
3. NEVER use broken HDD to store data due to potential data loss. This HDD is unstable, causes system files errors on clean OS, and its future usage could cause complete failure and data loss.

Always make backups and check the state of your HDD, and the risk of data loss will be drastically decreased!
