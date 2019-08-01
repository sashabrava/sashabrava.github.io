---
layout: article
title:  "Automatic file change with Photoshop"
date:  2019-08-01 22:11:00 +0300
categories: mapillary images photoshop
description: "If you're into mapping, you can face a problem that some photos are a big blury or have wrong color temparature/white balance. Here is a guide how to fix it."
---
<a target="_blank" href="https://www.openstreetmap.org/">Mapillary</a> is a platform for uploading user-created street images. The platform has applications for different devices (IOS, Android).
However, some pictures, taken from hands or during bad lighting can look so-so and some fix won't make them any harm. Photoshop has a powerful scripting tool for such task.
it can easily happen that you mapped a rare region, however, your picture looks shaky and wiper is looking out from the bottom of the picture together with colours too grey because of dirty windscreen . Like at the picture below.

![Default picture]({{ site.url }}/assets/img/2019-08-01-mapillary-pictures-photoshop/default-pic.png)

Photoshop allow to create an Action and run it on the folder of images. Let's load a certain picture from the sequence, create new set of actions and a new action.

![New action]({{ site.url }}/assets/img/2019-08-01-mapillary-pictures-photoshop/new-action.png)

If you want to add running effects on your image, simply push a "record"  button of Action and make all the necessary changes on your pictures.
In my case these changes will be additing  Canvas size, Shake Reduction and Auto Contrast.

![Action operations]({{ site.url }}/assets/img/2019-08-01-mapillary-pictures-photoshop/action-operations.png)

Due to some experience of using scripting, it's better to save your set of actions before running. Since different circumstances 
(Photoshop catches error, electricity goes down and etc.) can make script stopped, and it can be non-trival task to recover some rare image effects.

Now, when the Action is created and saved, it's time to specify a folder for script. `File -> Scripts -> Image Processor` will open a necessary menu.

Select there images folder, location for saving pictures, options of output files and choose necessary Action.
Some operations are resource-consuming, and if you make them on 100+ pictures, it can be time-consuming as well. 
Therefore it makes sense to take care about your machine's cooling and avoid running other resource-consuming operations.

![Processing]({{ site.url }}/assets/img/2019-08-01-mapillary-pictures-photoshop/processing.png)