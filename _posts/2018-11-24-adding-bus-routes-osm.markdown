---
layout: article
title:  "Adding bus routes to OpenStreetMap"
date:   2018-11-24 15:31:00 +0300
categories: maps
description: "When it comes to editing OpenStreetMap, there is a lack of step-by-step articles about it."
---
If you decide to edit OSM (<a target="_blank" href="https://www.openstreetmap.org/">OpenStreetMap</a>), you can face an issue that it's hard to find user guides about specific topics. The same story happens with adding bus routes on <a target="_blank" href="https://www.openstreetmap.org/#map=12/52.4916/13.4160&layers=T">Transport layer</a>.

![Transport Map]({{ site.url }}/assets/img/2018-11-24-adding-bus-routes-osm/transport-map.png)

Bus (and any another type of route) is a relation, which includes roads and bus stops (or platforms). I will use <a target="_blank" href="https://www.openstreetmap.org/edit?editor=id">iD editor</a>  for that (although <a target="_blank" href="https://help.openstreetmap.org/questions/29462/correcting-bus-routes"> some comments</a> say that it's too hard). You can  create a new relation by choosing a road (or bus stop), scrolling its properties up to section `all relations` and choosing `new relation`.

![Adding a relation]({{ site.url }}/assets/img/2018-11-24-adding-bus-routes-osm/all-relations.png) 

I used Berlin and London bus routes as examples because they seem to be well-maintained by community. That's an example of how basic bus route can look like.

![Berlin bus route]({{ site.url }}/assets/img/2018-11-24-adding-bus-routes-osm/berlin-line.png)

If you want to add a bus stop, it's <a target="_blank" href="https://wiki.openstreetmap.org/wiki/Buses">recommended (section "Adding bus stops to the relation")</a> to add the role `platform`. However, Berlin and London public tranport use old `stop`. 

![Specifying a role]({{ site.url }}/assets/img/2018-11-24-adding-bus-routes-osm/role-stop.png)
 
If you create a new bus route (or edited an old one), your changes won't appear on the map immediately. It may take days (or even a week) for a layer to process your data. However, there are  alternatives to look at your changes - <a target="_blank" href="https://www.öpnvkarte.de/" >ÖPNVKarte</a> and <a target="_blank" href="http://www.flosm.de/en/publictransport.html">Flosm</a>. ÖPNVKarte seems to be the fastest one while Flosm shows the date when data was loaded.

You may have a question "Should I add bus route twice, from A point to B and from B to A?". The answer seems to be "yes" - however, A <=> B route can be specified as one relation, and it's better to take a look at other routes of your city in order not to create a mess. 

If you want to see a bus route on the map, you must add roads to relation. It's recommended to add bus stops and specify tags "from" and "where" in relation - but if you skip them, you'll still see your route on the map.
