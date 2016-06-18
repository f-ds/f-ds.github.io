---
published: false
title: Create and Publish vector data from scratch
layout: post
---
This post is about vector data **creation** and **publishing** in a web map.

Why this should be interesting? Aren't there enough good map creation online tools out of there?

Well, yes. But let's say that after having try all the coolest online maptools your impression is that none of them fits your requirements and you need more flexibility to design the dataset and the functionality of the web map... in that case this post could be interesting.

For the creation of the data the only tool required is QGis: we will draw our custom **features** (geometries with a spatial location) and we will save them as a shapefile.

For the publication of the data... well we must have first a server with some geospatial tools like GeoServer, PostGIS, GDAL up and running. This post will provide few hints and references about how to build a such environment, it will mostly assume that the reader knows what the above mentioned tools are.

# Let's make some vector data

First at all we need QGIS so download http://www.qgis.org/en/site/ and install it
With postgis we can create any geometries like Points, Lines and Polygons like we can do with Inkscape or corel Draw but the cool thing is that those geometries have a location in the space so we can easilly use them to build a map mixing it up with any other map API service.

Ok let's make the first geometry, it's easy to activate the QGIS drawing tools and start drawing a polygon but... 


[IMG]



WHERE are we drawing it? How the spatial location of that geometry can be managed?

well, now concepts like projections and reference systems could be introduced... but let's make the story short and start to build things! 
In QGIS we can quickly install a plugin that allow us to import in the QGIS Layers one from the most famous maps services.

For example OpenStreetMap:


[IMG]



So WHERE are we drawing it? We are drawing a polygon over the belpaese!


Or here around the colosseum


[IMG]


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