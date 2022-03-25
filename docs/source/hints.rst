Hints
=====

Module help
-----------

Use ``module_name help`` to show help.

.. code-block:: console

   $ r.in.gdal help
   Imports raster data into a GRASS raster map using GDAL library.

   Usage:
    r.in.gdal [-ojeflakcrp] input=name output=name
      [band=value[,value,...]] [memory=memory in MB] [target=name]
      [title=phrase] [offset=value] [num_digits=value] [map_names_file=name]
      [location=name] [table=file] [gdal_config=string] [gdal_doo=string]
      [--overwrite] [--help] [--verbose] [--quiet] [--ui]

   Flags:
     -o   Override projection check (use current location's projection)
     -j   Perform projection check only and exit
     -e   Extend region extents based on new dataset
     -f   List supported formats and exit
     -l   Force Lat/Lon maps to fit into geographic coordinates (90N,S; 180E,W)
     -a   Auto-adjustment for lat/lon
     -k   Keep band numbers instead of using band color names
     -c   Create the location specified by the "location" parameter and exit. Do not import the raster file.
     -r   Limit import to the current region
     -p   Print number of bands and exit

   Parameters:
              input   Name of raster file to be imported
             output   Name for output raster map
               band   Band(s) to select (default is all bands)
             memory   Maximum memory to be used (in MB)
                      default: 300
             target   Name of GCPs target location
              title   Title for resultant raster map
             offset   Offset to be added to band numbers
                      default: 0
         num_digits   Zero-padding of band number by filling with leading zeros up to given number
                      default: 0
     map_names_file   Name of the output file that contains the imported map names
           location   Name for new location to create
              table   File prefix for raster attribute tables
        gdal_config   GDAL configuration options
           gdal_doo   GDAL dataset open options

Module manual
-------------

Use ``g.manual module_name`` to display the manual page in the browser.

.. code-block:: console

   $ g.manual r.in.gdal

Module GUI
----------

You are not familiar with the command line or typing, and want to use the Graphical User Interface (GUI)?
Most modules will start the GUI when no options are given, but some that do not require any options will just run without opening the GUI.
In this case, use the ``--ui`` flag.

.. code-block:: console

   $ r.in.gdal --ui
   $ r.in.gdal
