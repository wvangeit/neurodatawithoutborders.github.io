=====================
**Processing folder**
=====================

Processing refers to intermediate analysis of the acquired data to make it more amenable to scientific analysis. The results of such analysis are stored in processing "modules". All modules reside in the *processing* folder. 

The neudodata format uses modules to store data for and represent the results of common data processing steps, such as
spike sorting and image segmentation, that occur before scientific analysis of the data. Modules store the data used by
software tools to calculate these intermediate results. Each module provides a list of the data it makes available, and
it is free to provide whatever additional data that the module generates. Additional documentation is required for data
that goes beyond standard definitions. All modules are stored in the **processing** folder.

Like TimeSeries, Modules are each have their own folder where they store their data. Also like TimeSeries, the name of a
Module folder is arbitrary (i.e., user-defined).  Each module has the following minimum structure:

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Names of the data interfaces offered by this module. For example,  |
|                               |            | [0]=“EventDetection”, [1]= “Clusters”, [2]= “FeatureExtraction”.   |
|                               |            | Interface descriptions are below                                   |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *source* (attr)             | text array | Names of the acquisition and stimulus TimeSeries directly used by  |
|                               |            | the module as well as modules that this module has used data from  |
|                               |            | (full paths – e.g., “/acquisition/timeseries/shank_2”)             |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *module_description* (attr) | text       | Human-readable description of module and contents, for             |
|                               |            | documentation and end-user information                             |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *neurodata_type* (attr)     | text       | The string "Module"                                                |
+-------------------------------+------------+--------------------------------------------------------------------+

Modules have subfolders for each interface that is a part of the module, as well as any additional custom datasets and
subfolders that are stored in the module. The interface list provides a machine-readable way to identify what
information a module provides. Interfaces are not required to publish data and included interfaces can extend beyond
what is listed here, so long as the data made available through the new interface is documented. To facilitate software
identification of specific processing modules, the name of the module, preceded by the string “software:” (e.g.,
“software:kwik”) should also be in the interfaces list, even if all data published by the module is described by other
interfaces (e.g., Clustering, FeatureExtraction, UnitTimes, etc).

Modules themselves have user-defined names and they are free to contain arbitrary user-defined data. Data from the
interfaces published by a module are stored in folders named after the interface. The user is free to add additional
data to these interface folders.

The source field provides information that allows moving backward through the processing stream to find the source of data. Storage of additional provenance information (e.g., parameters, user decisions, etc) is left to the module's developer, as the types of provenance information can vary greatly between modules.

=================
Module Interfaces
=================

Several module interfaces are listed. When a module provides an interface name in its interfaces list, it is required to have the data fields listed with that interface. The data published by each interface should reside in a folder in the module having the same name as the interface.

EventDetection
--------------
Detected spike events from voltage trace(s). 

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "EventDetection"                          |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **EventDetection**          | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *times*                   | double     | Description of how events were detected, such as voltage           |
|                               | array      | threshold, or dV/dT threshold, as well as relevant values          |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *source_idx*              | int array  | Optional (relevant only if raw data is available).                 |
|                               |            | Indices into source *TimeSeries::data* array corresponding to      |
|                               |            | time of event. Module description should define what is event time |
|                               |            | (e.g., .25msec before action potential peak, zero-crossing time,   |
|                               |            | etc). The index points to each event from the raw data             |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + source_timeseries         | text       | Name and path (e.g., "acquistion/tetrode_4") of *ElectricalSeries* |
|                               |            | that this data was calculated from. Metadata about electrodes and  |
|                               |            | position can be read from that *ElectricalSeries* so it's not      |
|                               |            | necessary for that information to be stored here                   |
+-------------------------------+------------+--------------------------------------------------------------------+

EventWaveform
-------------
Represents either the waveforms of detected events, as extracted from a raw data trace in /acquisition, or the event waveforms that were stored during experiment acquisition.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "EventWaveform"                           |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **EventWaveform**           | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<SpikeEventSeries>*      | TimeSeries | One of possibly many *SpikeEventSeries*. Name(s) should be         |
|                               |            | informative                                                        |
+-------------------------------+------------+--------------------------------------------------------------------+

FeatureExtraction
-----------------
Features, such as PC1 and PC2, that are extracted from signals stored in a SpikeEvent TimeSeries or other source. 

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "FeatureExtraction"                       |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **FeatureExtraction**       | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *features*                | float      | Multi-dimensional array of features extracted for each event.      |
|                               | array      | *Array structure: [num events][num channels][num features]*        |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *times*                   | double     | Times of events that features correspond to (can be a link).       |
|                               | array      | Array structure: [num events]                                      |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *description*             | text array | Description of features (e.g., "PC1") for each of the extracted    |
|                               |            | features. *Array structure: [num_features]*                        |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *electrode_idx*           | int array  | Indices to electrodes described in the experiment's electrode map  |
|                               |            | array, in /general/extracellular_ephys.                            |
|                               |            | *Array structure: [num channels]*                                  |
+-------------------------------+------------+--------------------------------------------------------------------+

*Note: Present feature-extraction module doesn't support masking on probes/shanks with high electrode count.*

FilteredEphys
-------------
Ephys data from one or more channels that has been subjected to filtering. Examples of filtered data include Theta and Gamma (LFP has its own interface). FilteredEphys modules publish an ElectricalSeries for each filtered channel or set of channels. The name of each ElectricalSeries is arbitrary but should be informative. The source of the filtered data, whether this is from analysis of another time series or as acquired by hardware, should be noted in each's TimeSeries::description field. There is no assumed 1::1 correspondence between filtered ephys signals and electrodes, as a single signal can apply to many nearby electrodes, and one electrode may have different filtered (e.g., theta and/or gamma) signals represented.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "FilteredEphys"                           |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **FilteredEphys**           | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<ElectricalSeries>*      | TimeSeries | One of possibly many *ElectricalSeries* storing DSP filtered data. |
|                               |            | Name(s) should be informative                                      |
+-------------------------------+------------+--------------------------------------------------------------------+

LFP
---
LFP data from one or more channels. The electrode map in each published ElectricalSeries will identify which channels are providing LFP data. Filter properties should be noted in the ElectricalSeries description or comments field. 

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "LFP"                                     |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **LFP**                     | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<ElectricalSeries>*      | TimeSeries | One of possibly many *ElectricalSeries* storing LFP data.          |
|                               |            | Name(s) should be informative                                      |
+-------------------------------+------------+--------------------------------------------------------------------+


BehavioralEvents
----------------
The objective of the Behavioral interfaces is to provide generic hooks 
for software tools/scripts. This allows a tool/script to take the output 
one specific interface (e.g., UnitTimes) and plot that data relative 
to another modality data (e.g., behavioral events) without having to 
define all possible modalities in advance. 
*BehavioralEvents* is used to represent irregular events.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "BehavioralEvents"                        |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **BehavioralEvents**        | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<TimeSeries>*            | TimeSeries | One of possibly many *TimeSeries* storing event data. Name(s)      |
|                               |            | should be informative and *TimeSeries* descriptions should state   |
|                               |            | what the data represents. E.g., "Right/left decision (right=1,     |
|                               |            | left=0)"                                                           |
+-------------------------------+------------+--------------------------------------------------------------------+


BehavioralEpochs
----------------
*BehavioralEpochs* use *IntervalSeries* to store interval data.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "BehavioralEpochs"                        |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **BehavioralEpochs**        | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<IntervalSeries>*        | TimeSeries | One of possibly many *IntervalSeries* storing event data. Name(s)  |
|                               |            | should be informative and *TimeSeries* descriptions should state   |
|                               |            | what the data represents. E.g., "Rat is feeding in compartment     |
|                               |            | (1), rat is at food dish (2)"                                      |
+-------------------------------+------------+--------------------------------------------------------------------+


BehavioralTimeSeries
--------------------
*BehavioralTimeSeries* is for continuous data.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "BehavioralTimeSeries"                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **BehavioralTimeSeries**    | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<TimeSeries>*            | TimeSeries | One of possibly many *TimeSeries* storing event data. Name(s)      |
|                               |            | should be informative and *TimeSeries* descriptions should state   |
|                               |            | what the data represents. E.g., "Whisker angle, in radians."       |
+-------------------------------+------------+--------------------------------------------------------------------+


Position
--------
Position data, whether along the x, x/y or x/y/z axis. 

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "Position"                                |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **Position**                | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<SpatialSeries>*         | TimeSeries | One of possibly many *SpatialSeries* storing event data. Name(s)   |
|                               |            | should be informative and *TimeSeries* descriptions should state   |
|                               |            | what the data represents. E.g., "Red LED"                          |
+-------------------------------+------------+--------------------------------------------------------------------+


EyeTracking
-----------
Eye-tracking data, representing direction of gaze. 

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "EyeTracking"                             |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **EyeTracking**             | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<SpatialSeries>*         | TimeSeries | One of possibly many *SpatialSeries* storing direction of gaze.    |
|                               |            | Name(s) should be informative                                      |
+-------------------------------+------------+--------------------------------------------------------------------+


PupilTracking
-------------
Eye-tracking data, representing pupil size.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "PupilSize"                               |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **PupilSize**               | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<TimeSeries>*            | TimeSeries | One of possibly many *TimeSeries* storing pupil size.              |
|                               |            | Name(s) should be informative                                      |
+-------------------------------+------------+--------------------------------------------------------------------+


CompassDirection
----------------
With a CompassDirection interface, a module publishes a SpatialSeries object representing a floating point value for theta. The SpatialSeries::reference_frame field should indicate what direction corresponds to “0” and which is the direction of rotation (this should be “clockwise”). The si_unit for the SpatialSeries should be “radians”  or “degrees”.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "CompasDirection"                         |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **CompasDirection**         | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *<SpatialSeries>*         | TimeSeries | One of possibly many *SpatialSeries* storing direction.            |
|                               |            | Name(s) should be informative                                      |
+-------------------------------+------------+--------------------------------------------------------------------+


UnitTimes
---------
Event times in observed units (eg, cell, synapse, etc). The UnitTimes folder contains a folder for each unit. Name of the folder should match value in source module, if that is possible/relevant (e.g., name of ROIs from Segmentation module).

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "UnitTimes"                               |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **UnitTimes**               | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *unit_list*               | text array | List of units present                                              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *source*                  | text array | Name, path or description of how unit times were calculated        |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + **<unit_N>**              | folder     | Folder stores data about a particular unit. Folder name is unit    |
|                               |            | name or label                                                      |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + *times*                 | double     | Spike times for the unit (exact or estimated)                      |
|                               | array      |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + *unit_description*      | text       | Description of the unit (e.g., cell type)                          |
+-------------------------------+------------+--------------------------------------------------------------------+


Clustering
----------
Clustered spike data, whether from automatic clustering tools (e.g., klustakwik) or as a result of manual sorting. A Clustering module publishes the following datasets:

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "Clustering"                              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **Clustering**              | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *times*                   | double     | Times of clustered events, in seconds. This may be a link to the   |
|                               | array      | *times* field in an associated *FeatureExtraction* module.         |
|                               |            | *Array structure: [num events]*                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *num*                     | int array  | Cluster number for each event                                      |
|                               |            | *Array structure: [num events]*                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *source*                  | text       | Path to *FeatureExtraction* module that was the source of the      |
|                               |            | clustered data. Can be path to self.                               |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *cluster_nums*            | int array  | List of cluster numbers that are a part of this set (cluster       |
|                               |            | numbers can be non-coninuous)                                      |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *description*             | text       | Description of clusters or clustering (e.g., cluster 0 is          |
|                               |            | electrical noise, clusters curated using Klusters, etc)            |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *peak_over_rms*           | float      | Maximum ratio of waveform peak to RMS on any channel in the        |
|                               | array      | cluster. This provides for a basic clustering metric.              |
|                               |            | *Array structure: [num events]*                                    |
+-------------------------------+------------+--------------------------------------------------------------------+


ClusterWaveforms
----------------
The mean waveform shape, including standard deviation, of the different clusters. Ideally, the waveform analysis should
be performed on data that is only high-pass filtered so to preserve the original waveform. This is stored separately from the Clustering interface because it is expected to require updating. For example, IMEC probes may require different storage requirements to store/display mean waveforms, requiring a new interface or an extension of this one. A ClusterWaveform module publishes the following datasets:

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "ClusterWaveforms"                        |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **ClusterWaveforms**        | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *waveform_mean*           | float      | The mean waveform for each cluster, using the same indices for     |
|                               | array      | each waveform as cluster numbers in the associated clustering      |
|                               |            | module. For example, the cluster stored in *Clustering::num[x]*    |
|                               |            | is stored in *ClusterWaveform::waveform_mean[x]*                   |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *waveform_sd*             | float      | Stdev of waveforms for each cluster, using the same indices as for |
|                               | array      | mean                                                               |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *waveform_filtering*      | text       | Filtering applied to data before generating mean/sd                |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *clusterwaveform_source*  | text       | Path to Clustering module that was the source of the clustered     |
|                               |            | data.                                                              |
+-------------------------------+------------+--------------------------------------------------------------------+

ImageSegmentation
-----------------
Stores pixels in an image that represent different regions of interest (ROIs). Pixels are stored in both lists and 2D maps representing image intensity
Pixel lists are here called masks, and masks are allowed to change with time (i.e., in case ROI is moving). All segmentation data is stored in a “segmentation” subfolder. Each ROI is stored in its own subfolder within ImageSegmentation, with the ROI folder containing a list of successive masks and the timestamp that marks the start time for each mask (i.e., when the mask began to be used). Successive masks within each ROI folder should be named “mask_0”, “mask_1”, “mask_2”, etc.
Also for masking neuropil.

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "ImageSegmentation"                       |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **ImageSegmentation**       | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + **<image_plane>**         | folder     | Folder is human-readable description of imaging plane              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + *description* (attr)    | folder     | Description of image plane, recording wavelength, depth, etc       |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + **<roi_name>**          | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + + *img_mask_0*          | 2D float   | ROI mask, represented in 2D ([y][x]) intensity image               |
|                               | array      |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + + *pix_mask_0*          | 2D int     | ROI mask, represented as list of pixels (x,y) that compose mask    |
|                               | array      |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + + + *weight* (attr)     | float      | Weight of each pixel                                               |
|                               | array      |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + + *start_time_0*        | double     | Time when this mask starts to be used                              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + + *roi_description*     | text       | Description of this ROI                                            |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + **reference_images**    | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + + *<ImageSeries>*       | TimeSeries | One of possibly many image stacks that the masks apply to. Each    |
|                               |            | can be one-element 'stack'                                         |
+-------------------------------+------------+--------------------------------------------------------------------+

Florescence
-----------
Florescence information about a region of interest (ROI). Storage hierarchy of florescence should be the same as for segmentation (ie, same names for ROIs and for image planes). 

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "Florescence"                             |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **Florescence**             | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *roi_source*              | text       | Path to *ImageSegmentation* module defining the ROIs listed here   |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + **<image_plane>**         | folder     | Folder is human-readable description of imaging plane              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + *description* (attr)    | text       | Description of image plane, recording wavelength, depth, etc       |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + **<roi_name>**          | TimeSeries | Florescence is stored as *TimeSeries*. *TimeSeries* name should    |
|                               |            | match that of ROI definition in *ImageSegmentation* module         |
+-------------------------------+------------+--------------------------------------------------------------------+

DfOverF
-------
dF/F information about a region of interest (ROI). Storage hierarchy of dF/F should be the same as for segmentation (ie, same names for ROIs and for image planes). 
DfOverF/

+-------------------------------+------------+--------------------------------------------------------------------+
| Group, attribute or dataset   | type       | Description and comments                                           |
| name                          |            |                                                                    |
+===============================+============+====================================================================+
| **<module name>**             | folder     | Module name is arbitrary but should be informative                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + *interfaces* (attr)         | text array | Array contains the entry "DfOverF"                                 |
+-------------------------------+------------+--------------------------------------------------------------------+
| + **DfOverF**                 | folder     |                                                                    |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + *roi_source*              | text       | Path to *ImageSegmentation* module defining the ROIs listed here   |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + **<image_plane>**         | folder     | Folder is human-readable description of imaging plane              |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + *description* (attr)    | text       | Description of image plane, recording wavelength, depth, etc       |
+-------------------------------+------------+--------------------------------------------------------------------+
| + + + **<roi_name>**          | TimeSeries | dF/F is stored as *TimeSeries*. *TimeSeries* name should match     |
|                               |            | that of ROI definition in *ImageSegmentation* module               |
+-------------------------------+------------+--------------------------------------------------------------------+

DrOverR
-------
Reserved for intrinsic imaging; design pending

ImageProcessing
---------------
Design pending

