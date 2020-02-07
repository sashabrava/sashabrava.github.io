---
layout: article
title:  "Connecting Brother DCP-B7520DW MFP to Arch Linux via WiFi"
date:  2020-02-07 10:40:00 +0100
categories: [linux, arch, MFP]
description: "Guide how to connect Brother DCP-B7520DW MFP to Arch Linux"
---
It's relatively easy to connect modern multifunction printers to Windows systems. However, the same step on Linux systems can take more time and effort. This guide contains steps to connect Brother DCP-B7520DW to <a target="_blank" href="https://www.archlinux.org/">Arch Linux release 2020.02.01</a> system over WiFi.

There 2 available options: install all the components manually and install via Brother script. 

First install option starts from commands  `pacman -Sy` (updating package list), `pacman -S cups` (installing print server).

After the installation CUPS server should be launched via command `systemctl start org.cups.cupsd.service`. Optionally you can also enable its autostart by `systemctl enable org.cups.cupsd.service`.

Since now CUPS server can be accessed via address <a target="_blank" href="http://localhost:631">http://localhost:631</a>. By default it launches only on localhost and can't be accessed via your IP.

`lpadmin -p MyPrinter -E -v ipp://PRINTER_IP/ipp/print -m everywhere` is the main command to add printer to CUPS server. This command can have multiple additional parameters, which are described on <a target="_blank" href="https://www.cups.org/doc/man-lpadmin.html">man page</a>. Parameter `-m everywhere`  quieries printer to get PDD file, `-E` enables printer and lets it accept jobs. 

![CUPS start]({{ site.url }}/assets/img/2020-02-07-connect-brother-arch/cups-start.png)

The server is connected to printer, but printing is unavailable because of one package missing: ghostscript.

![CUPS error]({{ site.url }}/assets/img/2020-02-07-connect-brother-arch/ghostscript.png)

`pacman -S ghostscript` installs necessary package. After this step test page prints successfully.

![CUPS successful print]({{ site.url }}/assets/img/2020-02-07-connect-brother-arch/cups-success.png)

The printer is successfully connected, and we can start connecting scanner to the system. To do that, we need to install several packages.

`pacman -S dpkg` installs tool for driver .deb package installation.

`pacman -S sane`, installs command-line scan tool.

Now it's necessary to download scanner driver. It can be taken from <a target="_blank" href="https://support.brother.com/g/b/downloadlist.aspx?c=eu_ot&lang=en&prod=dcpb7520dw_eu&os=128">Brother support page</a>, there are both 32-bit and 64-bit versions. 
Driver can be installed via command `dpkg -i DOWNLOADED_FILE_PATH`.

`brsaneconfig4 -a name=YOUR_DEVICE_NAME model=YOUR_DEVICE_MODEL ip=YOUR_PRINTER_IP` connects scaner to the system. Paramaters `YOUR_DEVICE_NAME` and `YOUR_DEVICE_MODEL` seem to be necessary only as 'title' of the scanner inside the system.

`scanimage -L` command shows the list of available scanners in the system.

`scanimage --format=png --output-file test.png --progress` launches test scan and saves it as test.png file.

There are various options for <a target="_blank" href="https://wiki.archlinux.org/index.php/SANE#Install_a_frontend">scanning GUI</a>. One of them is skanlite.

`pacman -S skanlite` installs GUI scanning application.

![Skanlite]({{ site.url }}/assets/img/2020-02-07-connect-brother-arch/skanlite.png)

Second option of MFP installation is running Brother Driver Install Tool, which automatically installs printer driver, scaner driver and scaner key tool. However, the script v.2.2.1-1 didn't launch - it couldn't properly detect whether `cups`, `wget` and `dpkg` are installed on Arch Linux. Additionally it requires symbolic links in `/etc/init.d` folder for `cupsys`, `cups`, `lpd`, `lprbg` even if script is modified to bypass `cups`, `wget` and `dpkg` check, which made a little sense for me to continue script correction (because correct installation by such script can't be guaranteed). 

![Script unseccessful ]({{ site.url }}/assets/img/2020-02-07-connect-brother-arch/script-unsuccessful.png)
