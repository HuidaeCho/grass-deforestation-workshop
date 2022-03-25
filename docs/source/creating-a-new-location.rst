Creating a new location
=======================

Figuring out the projection of NLCD data
----------------------------------------

Any data providers should provide metadata about their data.
NLCD data sets also come with XML metadata files.
For example, for the 2001 NLCD data, we can find the nlcd_2001_land_cover_l48_20210604.xml file.
The ``<spref>`` tag in this metadata file is what we need.
NLCD uses its own custom projection.
`This search <https://spatialreference.org/ref/?search=nlcd>`_ will give a list of spatial references that mention NLCD.
All these spatial references use the Geodetic Reference System 1980 (GRS80) ellipsoid, but I found that NLCD has recently (not sure when) changed the ellipsoid from GRS80 to the World Geodetic System 1984 (WGS84).
These two ellipsoids are almost identical, but they are different.
To import NLCD data, we need to create a new location in the Albers Conical Equal Area projection with the WGS84 ellipsoid.
There are some more details in the metadata file.
We have two options:

#. manually construct projection parameters for GRASS or
#. use a GRASS module to create a new location in the NLCD projection.

Which method do I prefer?
Well... option (2) of course unless I know I already have a location in the desired projection.

There are three important modules in GRASS that can import raster data:

* r.in.gdal: Imports raster data into the current or a new location
* r.import: Imports and reprojects raster data on the fly into the current location
* r.external: Links external raster data sources as a pseudo raster map

Since the NLCD data sets we downloaded are huge (40 GB uncompressed), we just want to create links to these files using r.external and extract the Georgia area later for our study.
Both r.in.gdal and r.external provide the -j flag that checks the projection and exits.
However, only r.in.gdal can create a new location using the -c flag and the projection information.
We'll check the projection first.

Extract the ZIP files first.

.. code-block:: bash

   cd /users/USERNAME/downloads
   unzip Counties_Georgia.zip
   unzip nlcd_2001_land_cover_148_20210604.zip
   unzip nlcd_2019_land_cover_148_20210604.zip

Use r.in.gdal to check the projection of NLCD 2001.

.. code-block:: bash

   r.in.gdal -j input=nlcd_2001_land_cover_l48_20210604.img

outputs

.. code-block::

   Projection of dataset does not appear to match current location.

   Location PROJ_INFO is:
   name: Lat/Lon
   proj: ll
   datum: wgs84
   ellps: wgs84
   init: EPSG:4326

   Dataset PROJ_INFO is:
   name: Albers Conical Equal Area
   datum: wgs84
   ellps: wgs84
   proj: aea
   lat_0: 23
   lon_0: -96
   lat_1: 29.5
   lat_2: 45.5
   x_0: 0
   y_0: 0
   no_defs: defined

   Difference in: proj

Use v.in.ogr to check the projection of Counties_Georgia.shp.

.. code-block:: bash

   v.in.ogr -j input=Counties_Georgia.shp

outputs

.. code-block::

   Projection of input dataset and current location appear to match

Creating a new location for NLCD data
-------------------------------------

The projection of Counties_Georgia.shp is the same as that of the default location in EPSG:4326, which is not really *projected*.
Let's use the projection of NLCD because

#. it is *projected* and
#. its data size is too large to reproject.

We can create a new location using r.in.gdal.
Since creating a new location won't import any data, the output parameter's value doesn't matter.

.. code-block:: bash

   r.in.gdal -c input=nlcd_2001_land_cover_l48_20210604.img output=dummy \
                location=aea-nlcd-wgs84

Note that I used aea-nlcd-wgs84 as the location name.
This location name includes projection information, which will be useful later when you need to import other data in the same projection.

Switching the current location
------------------------------

Reload GRASS locations in the Data tab and switch to the PERMANENT mapset in aea-nlcd-wgs84.
