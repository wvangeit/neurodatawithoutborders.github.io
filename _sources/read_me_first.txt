================
Neurodata Format
================

The neurodata format ('nwb') is designed to store a variety of neurophysiology data, including from intra- and extracellular electrophysiology experiments, optophysiology experiments, as well as tracking and stimulus data. It has a defined schema and metadata labeling system designed so software tools can easily access the data.

NWB uses a schema that is implemented in an HDF5 file. HDF5 itself can be thought of as a file system residing in a file. There are two types of HDF5 objects, groups and datasets. Datasets are equivalent to files in a normal file system, and each stores a particular set of data. Groups are functionally equivalent to directories(folders). A group can store one or more files and/or groups. There are HDF5 libraries in many languages and platforms, and importantly, both python and Matlab support it.

NWB file contents are organized into several top-level groups, each storing data of a particular type, and top-level
datasets, storing basic information about the NWB file.


HDF5 overview
-------------

For the description that follows, it is important to first discuss some of the characteristics of HDF5. HDF5 can be considered to be a file-system in a file. An HDF5 file is composed of groups and datasets. Groups correspond to filesystem folders or directories, and datesets correspond to files. 

Groups are organized into a hierarchical nature, just like folders (directories) in a file system. HDF5 has the convention of using the slash symbol "/" as a path separator. Each HDF5 group can have within it zero or more groups and/or datasets.

Similar to a filesystem, groups and datasets can be links, or shortcuts, to a group or dataset located elsewhere.

HDF5 has an ability that goes beyond what is available in many file systems, as all groups and datasets can have arbitrary attributes associated with them. These attributes can be of any type (e.g., a string, an integer, a floating point array, etc.).


Top-level groups (folders)
--------------------------

A neurodata file is broken up into the following categories, each stored as its own HDF5 group (folder):

+------------------+----------------------------------------------------------------------------------------------+
| Folder           | Description                                                                                  |
+==================+==============================================================================================+
| **general**      | Experimental metadata, including protocol, notes and description of hardware device(s)       |
+------------------+----------------------------------------------------------------------------------------------+
| **acquisition**  | Time series of data that are recorded from the system, including ephys, ophys, tracking,     |
|                  | etc                                                                                          |
+------------------+----------------------------------------------------------------------------------------------+
| **stimulus**     | Time series of data that is pushed into the system (eg, video stimulus, sound, voltage, etc) |
+------------------+----------------------------------------------------------------------------------------------+
| **epochs**       | Experimental intervals, such as sub-experiments having a particular scientific goal, trials  |
|                  | during an experiment, or epochs deriving from analysis of data                               |
+------------------+----------------------------------------------------------------------------------------------+
| **processing**   | The home for processing 'Modules'. Modules perform intermediate analysis of data that is     |
|                  | necessary to perform before scientific analysis, including spike clustering, image           | 
|                  | segmentation, tracking data                                                                  |
+------------------+----------------------------------------------------------------------------------------------+
| **analysis**     | Lab-specific and custom scientific analysis of data. There is no defined format for the      | 
|                  | content of this folder – the format is up to the individual user/lab                         |
+------------------+----------------------------------------------------------------------------------------------+

Top-level datasets
------------------

Each neurodata file has the following fields that help identify the file and the data within it:

+-------------------------+---------------------------------------------------------------------------------------+
| Dataset                 | Description                                                                           |
+=========================+=======================================================================================+
| **nwb_version**         | File version string. This will be the name of the format with trailing major, minor   |
|                         | and patch numbers (e.g., NWB-0.4.18)                                                  |
+-------------------------+---------------------------------------------------------------------------------------+
| **identifier**          | A unique text identifier for the file                                                 |
+-------------------------+---------------------------------------------------------------------------------------+
| **file_create_data**    | Date and time file was created, UTC (ISO 8601)                                        |
+-------------------------+---------------------------------------------------------------------------------------+
| **session_start_time**  | Date and time experiment was started was created, UTC (ISO 8601). This serves as      |
|                         | the reference time for data in the file. All timestamps in a neurodata file are       |
|                         | stored as seconds after this reference time                                           |
+-------------------------+---------------------------------------------------------------------------------------+

Documentation organization
--------------------------

There are separate chapters for each of the top-level groups. There are additional chapters
for *Modules* and *TimeSeries*. The structure of NWB is based upon *Modules* and *TimeSeries* objects. Understanding these is essential for understanding the neurodata format.

Documentation is stored as text, with the text itself formatted as ReStructuredText (rst). The documentation should be
readable directly, and there are also tools capable of converting it to other formats, such as HTML. In order to use
such tools, the documentation must be pulled from the NWB file and saved as text files. The tools are able to translate these text files to the desired alternate format.

