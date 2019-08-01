---
layout: article
title:  "Editing OpenStreetMap road relations in JOSM editor"
date:  2019-08-01 22:07:00 +0300
categories: maps
description: "OpenStreetMap sometimes lacks information being organised into relations, and it's quite visible among road relations of some countries."
---
JOSM is a very powerful tool for editing <a target="_blank" href="https://www.openstreetmap.org/">OpenStreetMap</a>. Comparing to current iD 2.15.4 Web editor, JOSM allows to create new relation very quickly and to get information about values of attributes for multiple objects.

Let's imagine a Task nr.1 - to find out, if all the parts of road from single road relation has filled in data about speed limits or the amount of lanes. 
In order to get this info, the first thing to do is to enable Overpass API - a powerful tool, which allow to avoid downloading the whole piece of map and to download only those objects, which satisfy certain requirements.
It can be enabled from the menu `View -> Expert Mode`
![Expert mode]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/expert-mode.png)

If we want to get objects, which are a part of certain relation, we should run query


![JOSM download query]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-download-query.png)
```
[out:json][timeout:25];    
rel(335406);
(._;>>;);
out;
```

If we take a look at the map, we'll see that except of way the relation consist also nodes. But, in order to get info about speed limit, we have to filter data we have on the map.
To do this, push `Ctrl + F` and type `type:way highway=* ` in the search field.

![JOSM search]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-search.png)

If we take a look at the menu in the right part of the screen, we'll see a menu with values. It's the same data you can see on the website itself. 

![Ways attributes]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/ways-attributes.png)

However, JOSM makes working with this data more flexible. You can choose several objects and see all the values they have with a counter, how many objects have a certain value.
One more great feature of this editor is changing attribute value for all selected objects.

![Change attribute]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/change-attribute.png)

JOSM also allows to check the relation on completeness. If you have such a gap in the relation, it probably means that some piece of road is missing. 
However, real life shows that things aren't so simple about direction of the road - some roads have one piece of map chosen as forward, and next to him as backward.

![Relation completeness]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/relation-completeness.png)


Let's imagine a task Nr. 2 -  a task to create a new relation of the road and to add existing ways into it. To start editing, we need to download a piece of map. However, there is a more convenient way to get data.
JOSM query wizard allows to download only filtered data from certain region. Example below shows how to download only roads from a big region.

![JOSM query wizard]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-query-wizard.png)

```
[out:xml][timeout:90];
{{geocodeArea:Byerazino District}}->.searchArea;
(
  node["highway"](area.searchArea);
  way["highway"](area.searchArea);
  relation["highway"](area.searchArea);
);
(._;>;);
out meta;
```

`highway=* in 'Byerazino District'`

Now we need to select only our road. Let's type `Ctrl + F` and filter data:

![JOSM search road]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-search-road.png)

Now let's push the button of new relation  ![JOSM search road]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-button-create-relation.png).

![JOSM new relation]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-new-relation.png)

Don't forget to sort nodes in the relation ![JOSM search road]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-button-sort-relation.png).

The only thing left until your changes become public is to upload data using a special point of menu. ![JOSM search road]({{ site.url }}/assets/img/2019-08-01-editing-road-relations-osm/josm-button-upload.png)