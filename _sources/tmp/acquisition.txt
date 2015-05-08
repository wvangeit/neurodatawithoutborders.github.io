======================
**Acquisition folder**
======================

The acquisition folder includes data that is measured from the experimental system, such as electrode data, microscope
images and video tracking. Data pushed into the system is stored in the stimulus folder.

Raw acquisition data will often be too bulky to include in the neurodata file, and it will also be stored read-only.
This is supported in the neurodata format through use of HDF5 links. Specifically, *TimeSeries::data* can be an HDF5
link to a dataset in a remote file. It is required that the remote dataset be formatted appropriately, however, with
appropriate attributes (see *TimeSeries* documentation).

The organization of the acquisition folder is as follows:


+-----------------------------+--------------+--------------------------------------------------------------------+
| Group, Dataset or Attribute | Type         | Description and comments                                           |
| name                        |              |                                                                    |
+=============================+==============+====================================================================+
| acquisition                 | folder       | Stores data recorded from the system                               |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + timeseries                | folder       | The *TimeSeries* objects storing recorded data                     |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + + <TimeSeries_X>          | *TimeSeries* | Zero or more *TimeSeries*. Name(s) should be meaningful            |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + images                    | folder       | Stores graphic documentation of experiment and setup               |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + + <image_X>               | binary       | Zero or more documentation photographs or movies. Name should be   |
|                             |              | meaningful                                                         |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + + + format (attr)         | text         | Format of image (e.g., png, tif, mpeg)                             |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + + + description (attr)    | text         | Human-readable description of image                                |
+-----------------------------+--------------+--------------------------------------------------------------------+

