---
published: false
title: ogr2ogr for vector data ingestion
layout: post
---
## Overview

GDAL is a great tool for geospatial raster data processing but it's also great for vector data.

A bunch of utilities of the GDAL framework are called OGR, those are vector oriented.


## ogrinfo

The `ogrinfo` utility is basically what `gdalinfo` is but for vector data.

~~~~~~~~~~~~
#$ ogrinfo --formats
Supported Formats:
  -> "ESRI Shapefile" (read/write)
  -> "MapInfo File" (read/write)
  -> "UK .NTF" (readonly)
  -> "SDTS" (readonly)
  [...]
~~~~~~~~~~~~~~~~~~~

With ``ogrinfo --formats`` we get the list of the supported formats.

In order to be supported some formats require a further GDAL build, f.e. in order to support **PostGIS** the option `--with-pg` must be provided to the configure step.

~~~~~~~~~~~~
#$ ./configure --with-pg=/path/to/pg_config
~~~~~~~~~~~~~~~~~~

you have to substitute **/path/to/pg_config** with your local postgres data dir, its location depends on how and where (OS) you installed postgres because wait... postgres properly installed is needed to run the build!

if you use `ogrinfo` against a dataset (a shapefile in this case) you get some general information

~~~~~~~~~~~~
#$ ogrinfo  Forest_Reserves.shp
INFO: Open of `Forest_Reserves.shp'
      using driver `ESRI Shapefile' successful.
1: Forest_Reserves (Polygon)

~~~~~~~~~~~~~~~~~

Not too much information in this case, we can read the name of the layer(s), its type and we also get proof that our shapefile works well (or at least that's not totally broken)

To know something also about the features which compose the layer use the option `-al`

~~~~~~~~~~~~~~~~~
#$ ogrinfo  -al  Forest_Reserves.shp
INFO: Open of `Forest_Reserves.shp'
      using driver `ESRI Shapefile' successful.

Layer name: Forest_Reserves
Geometry: Polygon
Feature Count: 119
Extent: (22.448111, -17.469306) - (33.678680, -8.589563)
Layer SRS WKT:
GEOGCS["WGS 84",
    DATUM["World Geodetic System 1984",
        SPHEROID["WGS 84",6378137.0,298.257223563,
            AUTHORITY["EPSG","7030"]],
        AUTHORITY["EPSG","6326"]],
    PRIMEM["Greenwich",0.0,
        AUTHORITY["EPSG","8901"]],
    UNIT["degree",0.017453292519943295],
    AXIS["Geodetic longitude",EAST],
    AXIS["Geodetic latitude",NORTH],
    AUTHORITY["EPSG","4326"]]
fid: Integer (9.0)
AREA: Real (33.31)
PERIMETER: Real (33.31)
PROD_FOR_: Real (19.0)
[...more fields...]
OGRFeature(Forest_Reserves):0
  fid (Integer) = 372
  AREA (Real) = 0.0519999999999999980000000000000
  PERIMETER (Real) = 1.5840000000000001000000000000000
  PROD_FOR_ (Real) = 19
  PROD_FOR_I (Real) = 18
  SITE_CODE (Integer) = 27019
  AREANAME (String) = Mbeleshi
  [...more fields...] 
  POLYGON ((28.885049819946289 -9.966395225174564,[...The rest of the geometry coordinates..]))

OGRFeature(Forest_Reserves):1
  fid (Integer) = 373
  [...]
OGRFeature(Forest_Reserves):2
  fid (Integer) = 374
  [...]
~~~~~~~~~~~~~~~~~~~~~~~~

here we have many more information like **Feature Count**, the **CRS** and **Extent**, the "schema" of the layer and the content of its feature.

This output is very informative but also extremely verbose:

* use the command parameters `-geom={YES/NO/SUMMARY}` and `-fields={YES/NO}` to hide the attributes or the geometries from the output

* use the command parameter `-sql statement` to filter (by content or spatially) the features.

Yes, the `-sql` parameter allows us to run **SQL** queries although the underlying datastore is not a relational database! This is very useful and it saves my back many times. But let's talk about this in the next section

## ogr2ogr

With ogr2ogr we can do many things:

* Convert almost any vector format to almost any other vector format

* Filter on the fly with SQL queries features from any dataset

* Use ogr2ogr as base component of an ingestion system / ETL process


