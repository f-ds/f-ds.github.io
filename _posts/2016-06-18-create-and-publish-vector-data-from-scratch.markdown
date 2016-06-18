---
published: false
title: Create and Publish vector data from scratch
layout: post
---
This post shows how to **create** *georeferenced vector data* from scratch an **publish** them as a *layer* on a web map.

* Aren't there enough good map creation online tools out of there? 
* Why should I learn how to do it from scratch? 
* Why should I have to deal with desktop GIS software and Linux servers instead of using a modern web map maker?

Well, if after having tried all the coolest online mapcreators you still feel that you need more control on the design of the data and the functionalities of the map... in that case this post could be interesting.

Let's break the process in 3 different steps:

1. **Layer creation** we only need for  QGIS and an internet connection: we will draw our custom vector **layer** built of many **features** (geometries with a spatial location) and we will save them as a shapefile.

2. **Layer publication** well, we must have first a server with some geospatial tools like GeoServer, PostGIS, GDAL up and running. This post will provide few hints and references about how to build a such environment, it will mostly assume that the reader knows what the above mentioned tools are.

3. **Update the published data** with QGIS and the WFS protocol we can update vector layers published on a remote host as it was hosted locally.

## Layer creation
### Let's draw some vector data

First at all we need QGIS so download http://www.qgis.org/en/site/ and install it
With postgis we can create any geometries like Points, Lines and Polygons like we can do with Inkscape or corel Draw but the cool thing is that those geometries have a location in the space so we can easilly use them to build a map mixing it up with any other map API service.

Ok let's make the first geometry, it's quite easy to activate the QGIS drawing tools and start drawing a polygon but... 

![Drawing a polygon with no background](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_no_bkg.png)

WHERE are we drawing it? How the spatial location of that geometry can be managed?

well, now concepts like projections and reference systems could be introduced... but let's make the story short and start to build things! 
In QGIS we can quickly install a plugin that allow us to import in the QGIS Layers one from the most famous maps services.

For example OpenStreetMap:

![Drawing a polygon over Italy using OSM as background](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_osm_as_bkg.png)

So WHERE are we drawing it? We are drawing a polygon over the belpaese!

Or here around the colosseum:

![Drawing a polygon over the colosseum](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_colosseum.png)

Ok I hope this overview have clarified what are we going to do so let's continue with 2 minutes tutorial.

Assuming that you have a recent QGIS release already installed and an internet connection:


1) Install the QGIS web plugin

2) Load the openstreetmap layer and zoom in to Lucca

3) Create a new vector layer using a Shapefile 

browsing the top menu select 

Layer -> Create Layer -> New Shapefile Layer ...

the press OK and type your output file name

4) Draw a polygon


# Build up the environment

# Publish your very first own layer!