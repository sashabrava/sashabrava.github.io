---
layout: article
title:  "Creating OpenStreetMap wiki template redirect link"
date:  2019-08-01 22:10:00 +0300
categories: [maps, osm, wiki]
description: "The guide how to fix an issue on OSM Wiki."
---
<a target="_blank" href="https://wiki.openstreetmap.org/wiki/Main_Page">OpenStreetMap Wiki</a> is a platform to fill in data about OSM map content, for collaboration, discussion and storing data about map completeness and mapping especialities of different places around the globe.
It is based on <a target="_blank" href="https://www.mediawiki.org/wiki/MediaWiki">MediaWiki</a> engine and supports most of its functionality, which means that wiki templates can be used as {% raw %} `{{Template:Templatename}}`{% endraw %}. 
Simplified form {% raw %}`{{Templatename}}`{% endraw %} is supported in some Wiki's by default, however, it's not a case for OpenStreetMap and you have to configure redirect yourself.

To do this, I'll choose page <a target="_blank" href="https://wiki.openstreetmap.org/wiki/Pl:Motorway_status">Pl:Motorway_status</a>,
which shows no redirect to <a target="_blank" href=" https://wiki.openstreetmap.org/wiki/Template:Pl:Motorway_status">Template:Pl:Motorway_status</a>.
I'll insert following text of the page:

`
#REDIRECT [[Template:Pl:Motorway_status]]
` 

Redirect is completed - if I open the first page, I can see following text.

![Redirect completed]({{ site.url }}/assets/img/2019-08-01-osm-wiki-redirect/redirect-completed.png)

You might be wondering if the issue worth discussion. I think yes, because if we look at some templates, 
<a target="_blank" href="https://wiki.openstreetmap.org/wiki/Template:FR:Map_status">Template:FR:Map_status</a>  has no redirect while another page, 
<a target="_blank" href="https://wiki.openstreetmap.org/wiki/Template:ES:Map_status">ES:Map_status</a> , has it. 
And the fact that all shown templates have different language means that such trick might appear no matter what's the language of page you edit.
