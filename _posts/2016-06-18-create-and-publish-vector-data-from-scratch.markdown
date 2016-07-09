---
published: false
title: Create and Publish vector data from scratch
layout: post
---
This post shows how to *create* a **georeferenced vector dataset** from scratch an *publish* it as a **layer** on a web map.

Basically it explain how to, in sequence

* Draw georeferenced geometries called *features** over a map background taken from one of the most popular free map tiles services
* Build up a simple geospatial infrastructure from scratch
* Use the WFS protocol to easilly keep the features updated

Perhaps you are now wondering...

* Aren't there enough good online maps-maker tools out of there? 
* Why should I learn how to do it from scratch? 
* Why should I have to deal with GIS software and Linux instead of simply use an online map maker?

Well, try to bring out the best from mapbox and cartodb but if after that you still need more control over the overall map creation or simply you want to avoid fees... check this post out!

##The whole process in 3 steps

As just said before, we're going to break the process in 3 main steps:

1. **Layer creation** We will create a **georeferenced vector dataset** built of many **features**exporting it as a shapefile.

2. **Layer publication** Let's use some cool DevOps tools to build up in 10 minutes a production Linux server with GeoServer, PostGIS, GDAL ready to host and serve our custom geospatial data.

3. **Layer update** with QGIS and the WFS protocol we can update vector layers published on a remote host as it was hosted locally.

## Layer creation
### Let's draw some vector data

First of all we need **QGIS** so [download](http://www.qgis.org/en/site/) and install it.

With QGIS we can create any `Geometry` like 

`Points`, `Lines` and `Polygons` 

as we can do with any other traditional vector desktop software like *Inkscape* or *corel Draw*... but the cool QGIS thing is that if those geometries have a location on the earth (for example they are the shape of a country or a neighborhood) we can automayically assign the coordinates to all the vertexes.

Since each `Geometry` represents a spatial **feature** we can also set a couple of `Attributes`.

### 1 minute quick-and-dirty  tutorial

It's quite easy to activate the QGIS drawing tools and start drawing a polygon but... 

<img title="CLICK TO OPEN !!! - Drawing a polygon with no background" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_no_bkg.png" width="50%">

...**Where** are we drawing it? How the spatial location of that geometry can be managed?

well, now we can start to talk for hours about things like map projections and reference systems... but let's make the long story short and build something quick...

In QGIS we can easilly install the OpenLayers plugin that allow us to set in the workbench a layer we use only as background:

Among the others we can use OpenStreetMap:

<img title="CLICK TO OPEN !!! - Drawing a polygon over Italy using OSM as background" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_osm_as_bkg.png" width="50%">

So answering to the previous question we are drawing a polygon over Italy!

In the example below a polygon around the colosseum is drawn:

<img title="CLICK TO OPEN !!! - Drawing a polygon over the colosseum" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_colosseum.png" width="50%">

###5 minutes tutorial ( maybe 10 )

Assuming that you have a recent QGIS release already installed and an internet connection:

1. **Install** the QGIS OpenLayers plugin:

   From the top menu select `Plugins` -> `Manage and Install Plugins...`

   In the Search input type `openlayer`

   <img title="CLICK TO OPEN !!! - How to install QGIS OpenLayers plugin" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_install_openlayer_plugin.png" width="50%">

2. The top menu `Web` is now available.

   Load the OpenStreetmap layer to set a QGIS workbench reference background to draw our features

   <img title="CLICK TO OPEN !!! - Activate the OpenLayers plugin" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_activate_openlayer.png" width="50%">

   and them **zoom-in** over the geographic feature you want to model (a country boundary, a river, a  street...)

I decided to model the Reinassence Fortification Walls of my wonderful city **Lucca**

   <img title="CLICK TO OPEN !!! - Zoom in over Lucca" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_lucca.png" width="50%">

3. Create a new vector layer using a **Shapefile** format.

   **HINT**: Read something about the ancient (Shapefile)[https://en.wikipedia.org/wiki/Shapefile] geospatial vector data format if you never heard about it.

   from the top menu select:

   `Layer` -> `Create Layer` -> `New Shapefile Layer ...`

   <img title="CLICK TO OPEN !!! - Create new Shapefile layer" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_create_new_shapefilelayer.png" width="50%">

   then press OK and type your output file name.

   Fill-in the information about the `Geometry Type` and `Attributes` (fields) in the following dialogue box as shown below:

   <img title="CLICK TO OPEN !!! - New layer dialogue box" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_new_layer_dialogue_mod.png" width="50%">

4. Draw a polygon

   <img title="CLICK TO OPEN !!! - Add a new feature drawing it" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_draw feature_mod.png" width="50%">

   <img title="CLICK TO OPEN !!! - Add attributes" src="https://raw.githubusercontent.com/f-ds/f-ds.github.io/master/public/img/make_data_edit_attributes.png" width="20%">

# Build up the environment

# Publish your very first own layer!