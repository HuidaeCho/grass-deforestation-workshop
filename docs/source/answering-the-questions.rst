Answering the questions
=======================

Using the attribute table
-------------------------

Now, we have two statewide forest change statistics from Eqs. :eq:`forest_change_rate` and :eq:`forest_change_rate_per_capita`, and can compare individual counties to these statistics in the attribute table of ga_counties.
Right click on the ga_counties layer and open the attribute table.
Sort the table by the forestrate0119 or forestratecap0119 column, and compare the results.

Using v.db.select
-----------------

We can use v.db.select.

.. code-block:: bash

   v.db.select map=ga_counties columns="NAME10,min(forestrate0119)"

This command will give you the worst county in terms of the deforestation rate, which is Gwinnett County.

.. code-block:: bash

   v.db.select map=ga_counties columns="NAME10,max(forestrate0119)"

This command will give you the best county in terms of the deforestation rate, which is Terrell County.

Similarly, the following two commands will give you the worst and best counties in terms of deforestation per capita, which are Clinch and Lincoln Counties, respectively.

.. code-block:: bash

   v.db.select map=ga_counties columns="NAME10,min(forestratecap0119)"
   v.db.select map=ga_counties columns="NAME10,max(forestratecap0119)"

How many counties are worse than the statewide average in percentage?

.. code-block:: bash

   v.db.select map=ga_counties column="count(*)" where="forestrate0119 < -0.28"

Total 73 counties are worse than the state average.

How many are better than the statewide average in percentage?

.. code-block:: bash

   v.db.select map=ga_counties column="count(*)" where="forestrate0119 > -0.28"

Total 86 counties are better than the state average.

There are total 159 counties in Georgia, so these two numbers add up correctly.

Similarly, how many counties are worse or better than the statewide average in square feet per capita?

.. code-block:: bash

   v.db.select map=ga_counties column="count(*)" where="forestratecap0119 < -213"

Total 74 counties are worse than the state average.

.. code-block:: bash

   v.db.select map=ga_counties column="count(*)" where="forestratecap0119 > -213"

Total 85 counties are better than the state average.
