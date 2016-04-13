---
published: true
title: build gdal 1.9.1 with python ECW and MrSID support
layout: post
---
This should be work as is for building previous and later (1.11.0) versions of gdal.

The steps needed are:

1. **Build libwcw for ECW**
1. **Download and configure mrsid**
1. **Build gdal with python ECW and MrSID**
1. **Check for the supported formats**

## 1) Build libecwj2 for ECW

~~~~~~~~~~~~~~~
#$ sudo add-apt-repository ppa:ubuntugis/ubuntugis-unstable ( unstable packages)
#$ sudo apt-get install build-essential gdal-bin
#$ cd /tmp
#$ wget http://meuk.technokrat.nl/libecwj2-3.3-2006-09-06.zip
#$ unzip libecwj2-3.3-2006-09-06.zip 
#$ cd libecwj2-3.3
#$ sudo ./configure 
#$ sudo make (wait)
#$ sudo make install
~~~~~~~~~~~~~~~~~~~~

## 2) Download and configure mrsid

1. Download rasterDSDK from https://www.lizardtech.com/developer (need for registration). Pay attention to download the proper 32bit or 64 bit version and use the closer gcc distribution to your gcc installer. Although the package are commented with "Supports RHEL" they work also debian.

1. Extract the archive in /usr/local/share/ and rename the root directory if the extracted archive mrsid_rasterDSDK. Of course you can extract it where you want and call the root directory as you prefer but we will use this path later so pay attention if you want to place the DSDK somewhere else

## 3) Build gdal with python ECW and MrSID

It's Important to run this before anything else

~~~~~~~~~~~~~~~
#$ sudo apt-get install python-numpy python-scipy 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

then 

~~~~~~~~~~~~~~~
#$ sudo apt-get install build-essential python-all-dev
#$ cd /tmp
#$ wget http://download.osgeo.org/gdal/gdal-1.9.1.tar.gz
#$ tar xvfz gdal-1.9.1.tar.gz
#$ cd gdal-1.9.1
~~~~~~~~~~~~~~~~~~~~~~~~

The usage of a different prefix can causes issues when building with python

~~~~~~~~~~~~~
#$ ./configure --with-python --with-ecw=/usr/local --with-mrsid=/usr/local/share/mrsid_rasterDSDK 
#$ make
#$ sudo make install
#$ sudo ldconfig (configure properly the LD_LIBRARY_PATH)
~~~~~~~~~~~~~~~~~~~

Add also the params

~~~~~~~~~
--with-jp2mrsid --libdir=/usr/local/share/mrsid_rasterDSDK/lib
~~~~~~~~~~~~

to the configure step in order to support jpeg2000 throught MrSID drivers and to proper complile. Seems that it is required on a 32bit OS

## 4) Check for the supported formats

~~~~~~~~~~~~
#$ gdalinfo --formats | wc -l 
   102
#$ gdalinfo --formats | grep SID
   MrSID (rov): Multi-resolution Seamless Image Database (MrSID)
   JP2MrSID (rov): MrSID JPEG2000
#$ gdalinfo --formats | grep ECW
   ECW (rw): ERDAS Compressed Wavelets (SDK 3.x)
   JP2ECW (rw+v): ERDAS JPEG2000 (SDK 3.x)
~~~~~~~~~~~~~~~~~~~
