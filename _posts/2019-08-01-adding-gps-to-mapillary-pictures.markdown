---
layout: article
title:  "Manually adding GPS to images for Mapillary using GeoSetter"
date:  2019-08-01 22:16:00 +0300
categories: mapillary images
description: "If you're into mapping, you can face a problem that your device doesn't have GPS or it's unprecise. Here is a guide how to handle it."
---
<a target="_blank" href="https://www.openstreetmap.org/">Mapillary</a> is a popular platform for uploading user-created street images. The platform has varoius applications for different devices (IOS, Android).
However, some people may record images on Digital Cameras, which often don't contain GPS. The solution for that is to use custom GPS Logger and later merge data. That's what we will do in this guide.

One of the ways to do it is to use <a target="_blank" href="https://www.geosetter.de/en/main-en/">GeoSetter</a> - a software to merge images with GPX track and and adjust a time delay.
You can add GPX track by pushing an "open" button in Tracks tab. If you opened the folder with images, you can merge them by choosing `Edit -> Syncronise with GPS Data Files`.
However, time on the camera and GPS logger wouldn't fit ideally, and different time adjustment settings (adjust time, timezone and etc.) will help to syncronise data.
![Syncronise time of images and GPX file]({{ site.url }}/assets/img/2019-08-01-adding-gps-to-mapillary-pictures/syncronise-gps.png)

After doing all the steps above the map will look like this.

![Redirect completed]({{ site.url }}/assets/img/2019-08-01-adding-gps-to-mapillary-pictures/gps-unprecise.png)

As we can see, the track isn't ideal. Different circumstances can influence on preciseness of GPS logger, and the quality of track can be low. However, there are better ways to handle the problem rather than manual editing GPS through Goesetter or Mapillary web interface, which can be tiring for hundreds of images.

![Mapillary web editor]({{ site.url }}/assets/img/2019-08-01-adding-gps-to-mapillary-pictures/mapillary-web.png)

Geosetter can create a GPX track from GPS data in images. It means that after original track syncronisation we can save only files with key GPS points(`Edit -> Save changes of selected images`), create GPX track from it and auto-set GPS if images between images using option "Interpolate regarding shoot tIme with last or next position".
If we look at the map now, we'll see the difference between red (original) track and blue, edited one.

![Track edited]({{ site.url }}/assets/img/2019-08-01-adding-gps-to-mapillary-pictures/gps-manual.png)

Such method can be way too complicated and excessive for good GPX files. However, if your track has sufficient inaccuracy, such trick can help to make data more valuable.