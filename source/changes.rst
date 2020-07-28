Changes
=======

crYOLO
^^^^^^

Version 1.7.5
*************

* Fix a problem that the training was not working when no filter (like Low pass or JANNI ) was used (Thanks to Andrea Nans).

* The config file value :guilabel:`max_box_per_image` is now adapted automatically to the given training data. If this value is lower than the number of labeled particles per micrograph, not all particle are taken into account during training. The automatic adaption ensures that it is high enough.

* Fix a crash of crYOLO when no particles were picked (Thanks to Grigory Sharov).

* Fix network architectures "crYOLO" and "YOLO".


Version 1.7.4
*************
* As the problem with freezing crYOLO at the end of the training occurred on too many machines, we decided to switch from multiprocessing to multithreading permanently. We removed the option :option:`--use_multithreading`.

Version 1.7.3
*************

* Add two environment variables to work around a problem when crYOLO freezes during training. See :ref:`Troubleshooting <cryolo-freeze-label>` for more details. (Thanks to Jafar Lie).

* Fix the following error when training with STAR files from relion 3.1 (Thanks to Sarah Piper):

    ::

        Line #31 (got 6 columns instead of 1)
        Line #32 (got 6 columns instead of 1)
        Line #33 (got 6 columns instead of 1)
        Line #34 (got 6 columns instead of 1)

* Fix the following error when training on multiple GPUs (Thanks to Sarah Piper):

    ::

        AttributeError: 'MultiGPUModelCheckpoint' object has no attribute 'anchors'

* Multi-GPU training now as good as single GPU training. :guilabel:`batch_size` now understood as number of batches per GPU.

Version 1.7.2
*************

* Fix the following error at the end of a training session (Thanks to Matthew H. Cahn):

    ::

        Traceback (most recent call last):
          File "<string>", line 1, in <module>
          File "/.../lib/python3.6/multiprocessing/spawn.py", line 105, in spawn_main
            exitcode = _main(fd)
          File "/.../lib/python3.6/multiprocessing/spawn.py", line 115, in _main
            self = reduction.pickle.load(from_parent)
          File "/.../lib/python3.6/multiprocessing/synchronize.py", line 110, in __setstate__
            self._semlock = _multiprocessing.SemLock._rebuild(*state)
        FileNotFoundError: [Errno 2] No such file or directory

Version 1.7.0
*************

* Now works on non-square data natively. Previous this release, it squeezed non-square images into square images.

* New resizing strategy: Given a single number as :guilabel:`input_size` in your :ref:`configuration file <config-file-label>`, crYOLO will scale the shorter image dimension to this :guilabel:`input_size` and the long dimension according the original aspect ratio. This is the recommended setting.

* New behavior when training on images with mixed aspect ratios when a single number as :guilabel:`input_size` in your :ref:`configuration file <config-file-label>` is given (as recommended): crYOLO will scale the shorter image dimension to the :guilabel:`input_size` and the long dimension according to the original aspect ratio. Every time the image is used during training, it will select a random square region (:guilabel:`input_size` x :guilabel:`input_size`) on this image. During prediction, it is applied onto the full image, without the need to select a square region.

    **WARNING: Models need retraining**

    With the new resizing strategy it is necessary to retrain models that were trained on
    non-square data with previous crYOLO versions.

* Now supports lists as input_size [height,width] (e.g. [1024,1400]). In this case each image will resized to this size independently of the true aspect ratio.

* Supports Gaussian Mixture Models (GMM) as a normalization option (experimental). It fits a 2 component GMM to the image data and normalizes according to the brighter component. This ensures that it always normalize with respect to ice. This option has to be specified in your :ref:`configuration file <config-file-label>`

* Add option :option:`--cleanup` to prediction and training. When used, it will delete filtered images after training/prediction.

* Add option :option:`--skip` to prediction. When used, it will skip images that were already picked (Thanks at Pranav Shah).

* Installation: Default installation channel is now conda-forge. This was necessary as numpy from anaconda froze in some occasions.

* Filtering is now magnitudes faster on parallel filesystems. On our cluster with BeeGFS we filter on one node (4 cores) 12000 K3 micrographs in 20 minutes! With 1.6.1 this needed more than 24 hours. Please see the note about  :ref:`using crYOLO on clusters <parallel-filesystem-label>`.


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

Version 1.3.6
*************

* Can now show images with multiple aspect ratios.
* Supports writing of STAR files.
* Fixed issue that the size distribution was only based on a single micrograph.

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
