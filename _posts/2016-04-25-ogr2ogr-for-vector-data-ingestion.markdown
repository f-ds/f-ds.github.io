---
published: true
title: ogr2ogr for vector data ingestion
layout: post
---
## Overview

GDAL is a great tool for geospatial raster data processing but it's also great for vector data.

A bunch of utilities of the GDAL framework are called OGR, those are vector oriented.


## ogrinfo

The [ogrinfo](http://www.gdal.org/ogrinfo.html) utility is basically what [gdalinfo](http://www.gdal.org/gdalinfo.html) is but for vector data.

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

With [ogr2ogr](http://www.gdal.org/ogr2ogr.html) we can do many things:

* Convert almost any vector format to almost any other vector format

* Filter on the fly with SQL queries features from any dataset

* Use ogr2ogr as base component of an ingestion system / ETL process

The basic structure of a `ogr2ogr` command is:

`ogr2ogr [parameters] -f [out_format] outLayer sourceLayer`

1. `[parameters] ` one or more params like **-append** or **-overwrite** 

2. `[out_format]` The Code of the output format, see [this table](http://www.gdal.org/ogr_formats.html)

3. `outLayer sourceLayer` the name of the layer and how to connect to it. 

Regarding to the last point

* if the **out** or **dst** layer is stored in a "file" datastore (like **Shapefile** or **MapInfo**) the absolute or relative FS location of the main file (**.shp** or **.tab**) must be provided.

* if the **out** or **dst** layer is stored in a DBMS a string with all the required parameters must be provided. Check [among all the supported formats](http://www.gdal.org/ogr_formats.html) the one you wanna use to know how to compose the connection string.

Let's see some examples:

### Data conversion / Ingestion

We want to convert a MapInfo layer (`.tab`) to a PostGIS layer

~~~~~~~~~~~~~~
#$ ogr2ogr -f PostgreSQL PG:"dbname='fds' host='localhost' port='5432' user='fds' password='fds'" rivers.tab`
~~~~~~~~~~~~~~~~~~~~~~~~

You can notice the **PostGIS connection string**, if the DBMS would be **OracleSpatial** we would have to write something like `OCI:fds/fds@localhost`.

Let's say that the `river.tab` contains only the european rivers and in a second step we wanna add to the same layer on PostGIS also the asian rivers we have in the shapefile `asianRivers.shp`:

~~~~~~~~~~~~~~
#$ mv asianRivers.* rivers.shp
#$ ogr2ogr -append -f PostgreSQL PG:"dbname='fds' host='localhost' port='5432' user='fds' password='fds'" rivers.shp
~~~~~~~~~~~~~~~~~~~~~~~~

The `mv asianRivers.* rivers.shp` command is used to change the name to the files which compose the shapefile, in this way ogr2ogr will automatically try to use the existing rivers layer in the PostGIS database.

### Data Ingestion with SQL Filtering

Ok, let's say that we have another shapefile, `worldRivers.shp` with all the rivers of the world and we want to append it to the previous created rivers layer on PostGIS and that we have to address the following issues:

* We want to keep the existing european and asian data since we suppose that they're more accurate than the world-based dataset. 

* The attributes schema of our existing layer on PostGIS is `[rivername(varchar), continent(varchar), lenght(integer)]` but the one in the `world.shp` is different `[rivername(varchar), continent(varchar), lenght(double), origin(string)]`. the latter has one more attribute and the `lenght` one is in a different format.

It's time to use the amazing `-sql` parameter of the **ogr2ogr** command and run:

~~~~~~~~~~~~~~~~~~
#$ mv worldRivers.* rivers.shp
#$ ogr2ogr -overwrite -f PostgreSQL PG:"dbname='fds' host='localhost' port='5432' user='fds' password='fds'" rivers.shp -sql "SELECT rivername, continent, cast(lenght as integer) FROM rivers WHERE continent NOT IN ('asia', 'europe')" -skipFailures
~~~~~~~~~~~~~~~~~~~~~~~~

with the supplied query `SELECT rivername, continent, cast(lenght as integer) FROM rivers WHERE continent NOT IN ('asia', 'europe')` we select to import only a subset of the fields in the source dataset, changing the type of the lenght one with the `cast()` SQL function. 

Relying on the `continent` field we pick only the features related to Africa, America and Oceania continents.

Also note the final `-skipFailures` parameter which allows ogr2ogr to continue with the process when it fails with a features.