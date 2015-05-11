===================
**Stimulus folder**
===================

Experimental stimuli are stored here, with stimuli here defined as any signal that is pushed into the system as part of the experiment (eg, sound, video, voltage, etc). 
Many different experiments can use the same stimuli, and stimuli can be re-used during an experiment. The stimulus group is organized so that one version of template stimuli can be stored and these be used multiple times. These templates can exist in the present file or can be HDF5-linked to a remote library file.

The stimulus folder is organized as follows:

+-----------------------------+--------------+--------------------------------------------------------------------+
| Group or dataset name       | Type         | Description and comments                                           |
+=============================+==============+====================================================================+
| stimulus                    | folder       | Stores data pushed into the system                                 |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + templates                 | folder       | Local or remotely stored  *TimeSeries* objects of stimuli          |
|                             |              | repeated between experiments, or within an experiment              |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + + <timeSeries_X>          | *TimeSeries* | Zero or more *TimeSeries*. Name(s) should be meaningful            |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + presentation              | folder       | Stimuli presented during an experiment                             |
+-----------------------------+--------------+--------------------------------------------------------------------+
| + + <timeSeries_X>          | *TimeSeries* | Zero or more *TimeSeries*. Name(s) should be meaningful            |
+-----------------------------+--------------+--------------------------------------------------------------------+

