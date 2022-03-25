Clipping data
=============

Setting the computational region
--------------------------------

In GRASS, there is a concept called computational region.
A computational region is a region where any raster computations occur.
It can save a lot of computational time especially when the full extent of data set is larger than the area of interest.
We'll set the computational region to the extent of Georgia.

.. code-block:: bash

   g.region -ap vector=ga_counties res=30

In this command, we use the ga_counties vector map to calculate the extent and ``res=30`` to set the correct resolution.
Without the ``-a`` flag, the extent will be set first and the resolution will be adjusted slightly.
With this flag, the resolution will be forced first and the extent will be adjusted later.
Since we know the resolution of NLCD data set, we want to force the correct resolution of 30 meters.
The ``-p`` flag will print the final settings.

Extracting NLCD data for Georgia
--------------------------------

The CONUS NLCD data sets cover the entire Continental United States (CONUS), but our area of interest is restricted to Georgia.
We want to extract only this area to make further analysis more efficient.
We can use r.mask and r.mapcalc.

.. code-block:: bash

   r.mask vector=ga_counties
   r.mapcalc expression="ga_nlcd2001=nlcd2001"
   r.mapcalc expression="ga_nlcd2016=nlcd2016"
   r.mask -r
