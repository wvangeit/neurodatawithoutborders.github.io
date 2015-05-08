=================
**Epochs folder**
=================

An experiment can be separated into one or many logical intervals, such as sub-experiments or trials. Those intervals
are called epochs and are represented in the epochs folder.
Each epoch can have zero or more *TimeSeries* associated with it, for example acquisition or stimulus. 
Associating a *TimeSeries* with an epoch is optional, and the number (and name) of these associated *TimeSeries* is
user-defined.
The epoch defines a window in each of these *TimeSeries* indicating where the *TimeSeries* overlaps with the epoch.


+-------------------------------+------------+--------------------------------------------------------------------+
| Group, Dataset and Attribute  | Type       | Description and comments                                           |
| names                         |            |                                                                    |
+===============================+============+====================================================================+
| epochs                        | folder     | Stores data about sub-experiment or trial intervals                |
+-------------------------------+------------+--------------------------------------------------------------------+
| + <epoch_N>                   | folder     | Zero or more epochs. Name should be informative                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + neurodata_type (attr)     | text       | The string "Epoch"                                                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + description               | text       |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + start_time                | double     | Start time of epoch, in seconds                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + stop_time                 | double     | Stop time of epoch, in seconds                                     |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + tags                      | text array | User-defined tags that help identify or categorize an epoch.       |
|                               |            | E.g., can describe stimulus or behavioral characteristics          |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + <timeseries_X>            | folder     | Reference for zero or more *TimeSeries* that overlap with epoch.   |
|                               |            | Name should be informative                                         |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + idx_start               | double     | First index in *TimeSeries::data* and *TimeSeries::timestamps*     |
|                               |            | that overlaps with this epoch                                      |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + count                   | double     | Number of entries in *TimeSeries::data* and                        |
|                               |            | *TimeSeries::timestamps* that overlap with this epoch              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + timeseries              | HDF5 link  | Link to *TimeSeries*. Target *TimeSeries* can be in acquisition    |
|                               |            | or stimulus folders, or anywhere else in file                      |
+-------------------------------+------------+--------------------------------------------------------------------+

