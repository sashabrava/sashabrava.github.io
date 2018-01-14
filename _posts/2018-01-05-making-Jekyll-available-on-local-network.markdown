---
layout: article
title:  "Making Jekyll available on local network"
date:   2018-01-05 16:53:00
categories: firstpost
---
After finding out that my Jekyll blog isn't mobile friendly, I decided to fix its styling and check it on real Android device (since the view from "Responsive Design Mode" button of Mozilla Firefox differed from the view on Android Chrome app). The easiest way for testing is to connect both server and Android device to the same network and connect to server from device by IP address.
`ip route get 8.8.8.8 | awk '{print $NF; exit}'`

![]({{ site.baseurl }}/assets/img/2018-01-05-making-Jekyll-available-on-local-network/ip.png) 

But the first trial gave me an error.

<img src="{{ site.baseurl }}/assets/img/2018-01-05-making-Jekyll-available-on-local-network/android-error.png" alt="" border="1" style="max-width:100% "/>

Well, I also found out that my firewall is fine. It is inactive by default from Ubuntu installation.

![]({{ site.baseurl }}/assets/img/2018-01-05-making-Jekyll-available-on-local-network/firewall-default.png)

In case your firewall denies all ports, you can paste following commands.

![]({{ site.baseurl }}/assets/img/2018-01-05-making-Jekyll-available-on-local-network/firewall-allowed.png)

The problem appeared to be among Jekyll settings - if you run a server by command `jekyll serve` , it will be visible only on `localhost:4000` and `127.0.0.1:4000`. So, even if you type `http://your-ip:4000` at the server computer, you will get "Unable to connect". The solution was found on <a target="_blank" href="https://stackoverflow.com/a/16608698">GitHub</a> - if you want Jekyll to be available outside localhost, the command should be `jekyll serve --host=0.0.0.0`.

<img src="{{ site.baseurl }}/assets/img/2018-01-05-making-Jekyll-available-on-local-network/android-success.png" alt="" border="1" style="max-width:100%;"/>

