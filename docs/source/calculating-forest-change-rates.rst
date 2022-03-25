Calculating forest change rates
===============================

Counting the number of forest cells by county
---------------------------------------------

First, we need to copy the ga_counties vector map from the PERMANENT mapset to the project mapset because the PERMANENT mapset is read-only and we cannot add new columns to the map.

.. code-block:: bash

   g.copy vector=ga_counties@PERMANENT,ga_counties

In this command, the first ga_counties refers to the existing map in PERMANENT and the second one to a new map that will be created in the current mapset.

.. code-block:: bash

   v.rast.stats map=ga_counties raster=ga_forest2001 column_prefix=forest2001 method=sum
   v.rast.stats map=ga_counties raster=ga_forest2019 column_prefix=forest2019 method=sum

Calculating the forest area in square miles by county
-----------------------------------------------------

So how big is each cell?
Or what is the resolution?
This command will print the current region (``-p``) and its resolution in meters (``-m``).

.. code-block:: bash

   g.region -pm

outputs

.. code-block::

   projection: 99 (Albers Conical Equal Area)
   zone:       0
   datum:      wgs84
   ellipsoid:  wgs84
   north:      1405920
   south:      905520
   west:       939180
   east:       1427310
   nsres:      30
   ewres:      30
   rows:       16680
   cols:       16271
   cells:      271400280

Read ``nsres`` (North-South resolution) or ``ewres`` (East-West resolution).
That's 30 meters, so the conversion factor is 0.000347491943 mi\ :sup:`2` (900 m\ :sup:`2`) per cell.

Add new columns to store the forest area and calculate these columns.

.. code-block:: bash

   v.db.addcolumn map=ga_counties \
                  columns="forest2001_sqmi double, forest2019_sqmi double"
   v.db.update map=ga_counties column=forest2001_sqmi \
               query_column="0.000347491943*forest2001_sum"
   v.db.update map=ga_counties column=forest2019_sqmi \
               query_column="0.000347491943*forest2019_sum"

Calculating the annual average forest change rate by county
-----------------------------------------------------------

Add a new column and calculate the forest change rate per year.

.. code-block:: bash

   v.db.addcolumn map=ga_counties columns="forestrate0119 double"
   v.db.update map=ga_counties column=forestrate0119 \
               query_column="(forest2019_sqmi-forest2001_sqmi)/forest2001_sqmi/18*100"

Calculating the average annual forest change rate in square feet per capita by county
--------------------------------------------------------------------------------------

For this calculation, let's add a new column again and calculate the per-capita forest change rate in square feet per year per person.

.. code-block:: bash

   v.db.addcolumn map=ga_counties columns="forestratecap0119 double"
   v.db.update map=ga_counties column=forestratecap0119 \
               query_column="(forest2019_sqmi-forest2001_sqmi)/totpop10*2.788e7/18"

Calculating the statewide forest change statistics
--------------------------------------------------

We can sum the forest2001_sqmi, forest2019_sqmi, and totpop10 columns.

.. code-block:: bash

   v.db.univar map=ga_counties column=forest2001_sqmi

outputs

.. code-block::

   Number of values: 159
   Minimum: 37.635809870501
   Maximum: 353.989347350214
   Range: 316.353537479713
   Mean: 163.940404513716
   Arithmetic mean of absolute values: 163.940404513716
   Variance: 5822.20787306518
   Standard deviation: 76.3033935881307
   Coefficient of variation: 0.465433727667465
   Sum: 26066.5243176808


.. code-block:: bash

   v.db.univar map=ga_counties column=forest2019_sqmi

outputs

.. code-block::

   Number of values: 159
   Minimum: 32.380689216512
   Maximum: 356.112870613887
   Range: 323.732181397375
   Mean: 155.550052009654
   Arithmetic mean of absolute values: 155.550052009654
   Variance: 5654.94308130134
   Standard deviation: 75.1993555909978
   Coefficient of variation: 0.483441532930702
   Sum: 24732.4582695351

.. code-block:: bash

   v.db.univar map=ga_counties column=totpop10

outputs

.. code-block::

   Number of values: 159
   Minimum: 1717
   Maximum: 920581
   Range: 918864
   Mean: 60928.6352201258
   Arithmetic mean of absolute values: 60928.6352201258
   Variance: 16051276953.0493
   Standard deviation: 126693.63422465
   Coefficient of variation: 2.07937751710546
   Sum: 9687653

From the above outputs, we found that the total forest areas in 2001 and 2019 in the state are 26,067 mi\ :sup:`2` and 24,732 mi\ :sup:`2`, respectively, and the total population in 2010 is 9,687,653 people.
It was not really intended, but we're using the total population in 2010, which is somewhere inbetween 2001 and 2019.
That's not too bad!
What is the annual average statewide forest change rate in percentage?
That is,

.. math::
   :label: forest_change_rate

   \newcommand\SI[2]{#1\,\mathrm{#2}}
   \frac{\SI{24,732}{mi^2}-\SI{26,067}{mi^2}}{\SI{26,067}{mi^2}}\times\frac{1}{\SI{18}{years}}\times 100\%=\SI{-0.28}{\%/year}.

What is the annual average statewide forest change rate in ft\ :sup:`2` per capita?
That is,

.. math::
   :label: forest_change_rate_per_capita

   \frac{\SI{24,732}{mi^2}-\SI{26,067}{mi^2}}{\SI{9,687,653}{people}}\times\SI{2.788\times10^7}{ft^2/mi^2}\times\frac{1}{\SI{18}{years}}=\SI{-213}{ft^2/year/capita}.
