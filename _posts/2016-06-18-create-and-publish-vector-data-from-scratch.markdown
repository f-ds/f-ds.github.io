---
published: false
title: Create and Publish vector data from scratch
layout: post
---
This post shows how to **create** a *georeferenced vector dataset* from scratch an **publish** it as a *layer* on a web map.

Perhaps you are now wondering...

* Aren't there enough good online maps-maker tools out of there? 
* Why should I learn how to do it from scratch? 
* Why should I have to deal with desktop GIS software and Linux servers instead of using a modern web maps-maker?

Well, if after having tried all the coolest online maps-maker you still feel that you need more control on the design of the data and the functionalities of the map... in that case this post could be worth to be read.

Let's break the whole process in 3 main steps:

1. **Layer creation** we only need for QGIS and an internet connection: we will draw our custom vector **layer** built of many **features** (geometries with a spatial location) and we will save them as shapefiles.

2. **Layer publication** well, we must have first a server with some geospatial tools like GeoServer, PostGIS, GDAL up and running. This post will provide few hints and references about how to build a such environment, but the reader must be aware of what the above mentioned tools are.

3. **Update the published data** with QGIS and the WFS protocol we can update vector layers published on a remote host as it was hosted locally.

## Layer creation
### Let's draw some vector data

First of all we need QGIS so [download](http://www.qgis.org/en/site/) and install it.
With postgis we can create any geometries like Points, Lines and Polygons as we can do with any other traditional vector desktop software like *Inkscape* or *corel Draw*.

But the cool thing is that if those geometries have a location on the earth (for example they are the shape of a country or a neighborhood) with QGIS we can easily assign the coordinates to the vertex.

Ok let's make the first geometry, it's quite easy to activate the QGIS drawing tools and start drawing a polygon but... 

![Drawing a polygon with no background](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_no_bkg.png){:height="50%"}.

...**Where** are we drawing it? How the spatial location of that geometry can be managed?

well, now we can start to talk for hours about things like map projections and reference systems... but let's make the story short and start to build things! 

In QGIS we can quickly install the OpenLayers plugin that allow us to import in the QGIS Layers one from the most famous maps services.

For example OpenStreetMap:

![Drawing a polygon over Italy using OSM as background](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_osm_as_bkg.png)

So answering to the previous question: we are drawing a polygon over Italy!

In the example below instead a polygon around the colosseum is drawn:

![Drawing a polygon over the colosseum](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_colosseum.png)

Ok, I hope this overview have clarified what are we going to do in this post...  let's continue with 2 minutes tutorial.

Assuming that you have a recent QGIS release already installed and an internet connection:

1. Install the QGIS OpenLayers plugin

![How to install QGIS OpenLayers plugin](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_install_openlayer_plugin.png)

2. Load the OpenStreetmap layer, used as reference background to draw our layers

![Activate the OpenLayers plugin](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_activate_openlayer.png)

and them zoom-in over the geographic feature you want to model (a country boundary, a river, a street...) I decided to zoom-in over my wonderful city **Lucca**

![Zoom in over Lucca](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_lucca.png)

3. Create a new vector layer using a Shapefile 

browsing the top menu select 

Layer -> Create Layer -> New Shapefile Layer ...

![Create new Shapefile layer](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_create_new_shapefilelayer.png)

--

then press OK and type your output file name

--

![New layer dialogue box](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_new_layer_dialogue_mod.png)

4) Draw a polygon

![Add a new feature drawing it](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_draw feature_mod.png)

--

![Add attributes](https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_edit_attributes.png)


# Build up the environment

# Publish your very first own layer!
