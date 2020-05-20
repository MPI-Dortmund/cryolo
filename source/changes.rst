Changes
=======

crYOLO
^^^^^^

Version 1.7.0
*************

* Now works on non-square data natively. Previous this release, it squeezed non-square images into square images.

* New resizing strategy: Given a single number as :guilabel:`input_size` in your :ref:`configuration file <config-file-label>`, crYOLO will scale the shorter image size onto this :guilabel:`input_size` and the long size according the aspect ratio. This is the recommended setting.

* New behavior when training on images with mixed aspect ratios when a single number as :guilabel:`input_size` in your :ref:`configuration file <config-file-label>` is given (as recommended): crYOLO will scale the shorter image size onto this :guilabel:`input_size` and the long size according the aspect ratio. Everytime the image is used within training, it will select a random square region (:guilabel:`input_size` x :guilabel:`input_size`) on this image. During prediction, it is applied onto the full image, without the need to select a square region.

.. warning::

    **Models need retraining**

    With the new resizing strategy it is necessary to retrain models that were trained on
    non-square data with previous crYOLO versions.

* Now supports lists as input_size [height,width] (e.g. [1024,1400]). In this case each image will resized to this size independent of the true aspect ratio.

* Supports Gaussian Mixture Models (GMM) as normalization option (experimental). It fits a 2 component GMM to you image data and normalizes according the brighter component. This ensures that it always normalize with respect to ice. This option has to be specified in your :ref:`configuration file <config-file-label>`

* Add option :option:`--cleanup` to prediction and training. When used, it will delete filtered images after training/prediction.


Version 1.6.1
*************

* Fixed a bug that was introduced with 1.5.5: Scaling of the anchor boxes was wrong. This leads to longer and unstable training and heavily affects the fine-tune mode. (Thanks to Jorge Jimenez de la Morena and Pablo Conesa)
* Fixed a bug that leads to an exception (_tkinter.TclError: couldn't connect to display) at the end of the training on cluster machines. (Thanks to Wolfgang Lugmayr)


Version 1.6.0
*************

* In case of the general model, you can specify with :option:`--minsize` MIN :option:`--maxsize` MAX a minimum and maximum size. This will filter the particles according to their estimated size.
* The estimated size and confidence distribution are now written in a new subfolder :file:`DISTR` in your output folder. It will also write .csv files with a summary of the distributions.
* In case of the general model, you don't need to specify the anchor size anymore.
* With every run, crYOLO now writes the command used into the central log directory.
* All log files (runfiles, commands, tensorflow) are now saved in the central log directory.
* During training, the intermediate models now get a suffix “_tmp”. After training is finished they are renamed to the specified name in the configuration file (field: “save_weights_name”).
* The boxmanager can now be started through the crYOLO GUI.
* Fixed issue that the filament mode does not work with micrographs that were motion-corrected by unblur.
* Fixed issue that the flaq :option:`--write_empty` did not work for the filament mode.
* Fixed issue that the minimum distance filter was not applied on particles in .cbox files.
* Fixed issue with the evaluation tool that crashed if no particle can be found for a specific threshold.



Boxmanager
^^^^^^^^^^

Version 1.3.5
*************

* Fixed a bug when placing, moving or deleting a box
* Fixed bug of nun closing progress dialog when writing boxfiles

Version 1.3.1
*************

* Speed up boxfile import is now 2x faster compared to 1.3.0.
* Big speed-up for live-preview during filtering. Should now even work with very big datasets.

Version 1.3.0
*************

* Added option to plot size- and confidence distribution for cbox files.
* Added slider to filter particles according their estimated size.
* Added addition field for the number of boxes with live update.
* Added wildcard commandline option.
* Show progress-bar when reading and writing box-files.
* Various speed-ups.