---
published: true
title: draft - raster algebra with GDAL (and no QGIS)
layout: post
---
I recently tried to use the **QGIS** 2.14.1 raster calculator to do some very basic operations on a forest loss map. 

The raster have a `~47000x46000` resolution with a `Byte` pixel type. The file size of the uncompressed **GeoTiff** version was about `2GB`.

Since my forest loss map represents the areas where forest has been lost in each year between 2001 and 2014 each "valorized" pixel contains an integer number p with `1 <= p <= 15` indicating when the loss happened.

My task was simple: create a single raster representing the forest loss status for each single year (so create 14 different files). This different format was needed for a further elaboration.

I opened QGIS soon and I used its **Raster calucator** functionality


`("italy_loss_2000-2014_retiled_lzw@1" ==9)*9+("italy_loss_2000-2014_retiled_lzw@1"!=9)*0`


Prerequisites

* A background about GIS and raster Algebra is needed (otherwise probably you aren't interested in this blogpost)
* For the hands-on section of this blogpost you have to be familiar with [Vagrant and desktop virtualization](http://f-ds.github.io/devops/2016/04/14/real-vagrant-in-2-minutes-run-ubuntu-or-centos.html)