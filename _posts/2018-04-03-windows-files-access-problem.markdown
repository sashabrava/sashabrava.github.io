---
layout: article
title:  "Moving files from one Windows 10 HDD to another"
date:   2018-04-03 18:30:00
categories: Windows
description: "One day there is a moment, when an old HDD needs to be replaced - the same as content structure inside HDD needs an update. But not everything will go smoothly on Windows 10."
---
One day there may happen a situation, when an old HDD needs to be replaced - the same as content structure inside HDD needs an update. But not everything will go smoothly on Windows 10.

If you attach an old HDD to freshly installed system, there can be an issue with accessing partitions and folders.

![Folder permissions]({{ site.url }}/assets/img/2018-04-03-windows-files-access-problem/folder-permissions.png)

If you try getting access to User folders, new Windows 10 still will ask you to give Administrator rights - althought you just surf through USB storage. However, Files Explorer won't be able to reach folder at all. CMD is a bit another story - it can access almost any folder, if you run it as Admin.

Even if File Manager says "Access denied".

![Access denied]({{ site.url }}/assets/img/2018-04-03-windows-files-access-problem/access-denied.png)

Althought, due to complicated access rights established on my old Windows, even Command Prompt wasn't able to read all the files. Until the group "Administrators" got an ownership and full permissions on the partition by editing "security" tab of properties.

![Success]({{ site.url }}/assets/img/2018-04-03-windows-files-access-problem/success.png)

![Partition permissions]({{ site.url }}/assets/img/2018-04-03-windows-files-access-problem/partition-permissions.png)
