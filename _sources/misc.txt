====================
Extending the format
====================

At a practical level, the NWB file is extended by users creating *TimeSeries* to store their data and adding the
requisite fields to interpret the data stored within it. 
This can be done with a simple *TimeSeries*, or users can take a more relevant series (e.g., *OpticalSeries*) when it
better matches their requirements. All *TimeSeries*, whether invented or taken from the specification can have custom
data fields added to it.
It is a point for discussion whether or how these extensions should be differentiated from data fields required by the
standard. One option, taken by the Allen Institute when releasing files in a draft of this format, was to prepend
"aibs" to each custom field. 

If an existing *TimeSeries* is 
Most extensions are expected to be done this way. 


The format is officially extended by adding new datasets to existing *TimeSeries* or *Modules* in the specification, or
defining new *TimeSeries* and/or *Interfaces*.  This level of extension is primarily to allow general software tools to
recognize the extensions, and it is expected to be done when an oversight committee recognizes new needs or popular
custom extensions.
When new *TimeSeries* or *Interfaces* extensions become popular, these are added to the format specification, while
existing *TimeSeries* and *Interfaces* persist. This approach allows extensibility without breaking compatibility.

==============
Data integrity
==============

HDF5 files can become corrupted if a write operation fails, such as through a tool/script crashing or being manually
closed when writing, or by power failure. This is not unique to HDF5, as other programs (e.g., MATLAB) can produce
corrupt files when they crash. A corrupted HDF5 file is effectively non-recoverable. 
There are strategies to deal with this but they have not been explored or implemented yet.

