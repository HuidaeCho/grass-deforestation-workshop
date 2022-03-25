Reclassifying NLCD data
=======================

NLCD data sets have many classes defined `here <https://www.mrlc.gov/data/legends/national-land-cover-database-class-legend-and-description>`_, but we're only interested in forest area.
There are three forest classes including 41, 42, and 43, but, in general, we can say any 40s classes are forest.
We can either extract these classes as 1 and the other classes as 0 using r.mapcalc, or reclassify them using r.reclass.
In most cases, reclassifying will be much faster than performing map algebra, so we want to use r.reclass.
First, let's reclassify the 2001 land cover map.
This module expects a rule file, but we'll just type in the console (a special filename -).
The "*" symbol means the rest of cell values, so we don't want to use it in the first line.

.. code-block:: bash

   r.reclass input=ga_nlcd2001 output=ga_forest2001 rules=-

Type the following rules in the prompt:

.. code-block::

   40 thru 49 = 1 forest
   * = 0 non-forest
   end

Then, repeat it for the 2019 map.

.. code-block:: bash

   r.reclass input=ga_nlcd2019 output=ga_forest2019 rules=-

Type the following rules in the prompt:

.. code-block::

   40 thru 49 = 1 forest
   * = 0 non-forest
   end
