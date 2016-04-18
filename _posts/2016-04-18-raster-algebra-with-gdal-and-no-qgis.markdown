---
published: true
title: Raster algebra with GDAL (and no QGIS)
layout: post
---
## The problem

I recently tried to use the **QGIS** 2.14.1 raster calculator to do some very basic operations on a forest loss map. 

The raster has a `~47000x46000` resolution with a `Byte` pixel type. The file size of the uncompressed **GeoTiff** version was about `2GB`.

Since my forest loss map represents the areas where forest has been lost in each year between 2001 and 2014 each "valorized" pixel contains an integer number p with `1 <= p <= 15` indicating when the loss happened.

My task was simple: create a single raster representing the forest loss status for each single year (so create 14 different files). This different format was needed for a further elaboration.

I opened QGIS soon and I used its **Raster calucator** functionality

![The QGis Raster Calculator GUI](https://rawgit.com/f-ds/f-ds.github.io/master/public/img/raster-calculator.jpg){: height="345px" width="500px"}

In order to extract the data for the year 2009 I wanna create a new raster with the same properties of the input one but with pixel value equals to 0 where the pixel in the input raster not equals to 9

`("italy_loss_2000-2014_retiled_lzw@1" ==9)*9+("italy_loss_2000-2014_retiled_lzw@1"!=9)*0`

But when I run the process I get this error:

![The QGis Raster Calculator OOM](https://rawgit.com/f-ds/f-ds.github.io/master/public/img/raster-calculator-error.jpg)

Before try to run the process with QGis 2.14.1 I tried also with a previous version, the result was that after a long time I got a non-sense output raster.
In the last QGIS release we get suddenly a clear message: I don't have enough memory to run this process, although I have about 4GB of free RAM space.

This behaviour seems to be confirmed by this [QGIS Feature Request](https://hub.qgis.org/issues/13336) and its related discussion below.

## The solution

So what can I do if I don't want wait for the implementation of this new feature request? The QGIS community is amazing but I need that work done ASAP and I can't wait for the developments nor contribute to QGIS atm.

QGIS use behind the scenes the **GDAL** library to perform its processing.

The GDAL utility `gdal_calc.py` is used to perform raster algebra operations, [have a look to its documentation](http://www.gdal.org/gdal_calc.html). As you can easilly see the command ends with the extension  **.py** so we need a GDAL installation **with python bindings** which is a set of higher level utilities written in python which use the core libraries written in C like **gdal_translate**, **gdalwarp** ecc..

In the next sections we are going step-by-step to create a GDAL build with pyton bindings building it from the sources. It is much more simple thatn it seems and there's no need of actual experience in C programming, just to follow some basic steps with the **make** utility.

Learn how to build GDAL could be very usefull for a GIS technician seems somethimes it's needed in order to use its brand new features or advanced configurations which are (still) not supported by QGIS.

Do you have `gdal_calc.py` already working on your machine? jump directly to **Run the processing with gdal_calc.py**

## Let's make our hands dirty

### Create the environment
The first thing to do is running a fresh installation of a Ubuntu distro, let's say Ubuntu precise. In my village we say: "old chicken makes a tasty broth". We can use also a non-fresh installation of course but... Why waste time with broken dependencies and other system missconfiguration when we have amazing tools like **Vagrant** and **VirtualBox**?

Follow this 2 minutes tutorial [Vagrant and desktop virtualization](http://f-ds.github.io/devops/2016/04/14/real-vagrant-in-2-minutes-run-ubuntu-or-centos.html) and come back here when your Ubuntu is up and running.

### Build GDAL with pyton bindings

TODO

### Run the processing with gdal_calc.py

TODO