Creating a project mapset
=========================

We have imported all the raster and vector maps that we need for our study into the same location that can be used for any other projects as well.
Now, we want to isolate these "clean" maps from other maps that we’ll create later for our analysis.
This way of map organization is just my personal preference to separate out "project-specific" data from "global" data in the PERMANENT mapset.
Create a new mapset called "ga-deforestation" in the aea-nlcd-wgs84 location.
You will automatically be switched to the new mapset.

Restrict the computational region to Georgia using the ga_nlcd2001 raster.

.. code-block:: bash

   g.region -p raster=ga_nlcd2001
