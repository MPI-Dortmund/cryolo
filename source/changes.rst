Changes
=======

crYOLO
^^^^^^

Version 1.9.1
*************

* New boxmanager based on napari!
* Updated all tutorials for the new napari boxmanager
* New cryoSPARC output format makes importing particles/filaments in cryoSPARC more easy.
* New RELION filament star files with precalculated priors
* The cryolo_boxmanager_tools 1.5 now provides two new tool commands:
    * :guilabel:`cryolo_boxmanager_tools.py createAutopick` to create the necessary autopick.star for extraction in relion 4. See our :ref:`tutorial <import-relion-4-label>` for more information.
    * :guilabel: With `cryolo_boxmanager_tools.py class2Dextract` you can extract the coordinates of good 2D classes from RELION to make them usable for training with crYOLO.
* Validity check for lowpass filtered images. For example, if you change the input size in your config.json, you had to delete the filtered images directory manually before. This now happens automatically.
* Old cryolo_boxmanager.py now is cryolo_boxmanager_legacy.py.
* Faster lowpass filter because of more ideal size estimation for FFT.
* Improved windows compatibility (Picking Tomograms + Filament picking does not work yet...).  (Thanks Jessica Elise Heebner)
* For CUDA11 setup: Use nvidia-tensorflow 1.15.5+nv22.2
* Fix imageio dependency (Thanks to Grigory Sharov)
* Fix broken setup instructions (Version setuptools must be < 66)


Version 1.8.4
*************

* Fix dependency problem of protobuf (Thanks to Pranav Shah). This error leads to following exception when running cryolo:

::

    TypeError: Descriptors cannot not be created directly.
    If this call came from a _pb2.py file, your generated code is out of
    date and must be regenerated with protoc >= 3.19.0.
    If you cannot immediately regenerate your protos, some other possible
    workarounds are:
     1. Downgrade the protobuf package to 3.20.x or lower.
     2. Set PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python (but this will use pure-Python parsing and will be much slower).


* Filament tracing now uses a chunksize of 1 and should use the processor more efficiently.


Version 1.8.3
*************

* Fix dependency problem of pystardb. Now 0.3.1 instead of 0.4 is installed.

Version 1.8.2
*************

* Fix problem that cryolo uses more CPUs then specified by --nc (Thanks to Jose Miguel de la Rosa Trevin)
* Fix GLIBC installation problem with CUDA 11

Version 1.8.1
*************

* Fix crash when cryolo_evaluation is used with CBOX files as ground truth.

Version 1.8.0
*************
* Adds a picking mode for tomography that works with single particles and filaments.
* Increased filament support: crYOLO now learns end-to-end to estimate the filament direction that is used during tracing. In previous versions this was done using a rotating convolutional mask which is rather slow. Moreover, the old method runs into problem if a filament only limited line features.

    **WARNING: Models need retraining**

    If you want to use the feature with your old filament models, you need to retrain them.
* For filaments, crYOLO creates two additional folders: :file:`CBOX` and :file:`CBOX_FILAMENT_SEGMENTED`. While the first folder  contains the picked particles from crYOLO which are input for filament tracing, the second new folder (:file:`CBOX_FILAMENT_SEGMENTED`) contains segmented filaments in CBOX format which also allow live filtering via the confidence threshold in the BoxManager
* Add new data augmentation (Full random rotation besides flipping).
* crYOLO 1.8 comes with several library updates. This is part of the ongoing transition to tensorflow 2:
    * Cuda 9 -> Cuda 10 / Cuda 11
    * Tensorflow 1.10.1 -> Tensorflow 1.15.4
    * NumPy 1.14.5 -> NumPy 1.18.5
    * Keras 2.2.5 -> Keras 2.3.1
    * wxpython 4.0.1 -> 4.1
    * mrcfile 1.1.2 -> 1.3.0 (Thanks to Miguel Esteva)
* CBOX files are now written in the STAR format.
* Now crYOLO allows to use .star files as input during prediction. crYOLO will pick all micrographs in the column '_rlnMicrographName'. As the path in this column is relative to your project directory, you need to start crYOLO from your project directory.
* crYOLO BoxManager is updated to 1.4.0
* Submit to queuing system directly from the crYOLO GUI (Thanks to Nicolas Ballet)
* Better support for new Relion (>=3.1) STAR files (Thanks to Grigory Sharov)

Version 1.7.7
*************
* Fixed issue with end of line character in filament STAR (START-END) files (Thanks to Grigory Sharov)
* Fixed a recursion depth error for filament tracing (Thanks for Grigory Sharov)

Version 1.7.6
*************
* Fixed library issue. (Thanks to Grigory Sharov)

Version 1.7.5
*************

* Fixed a problem where the training was not working if no filter (like Low pass or JANNI ) was used. (Thanks to Andrea Nans)

* The config file value :guilabel:`max_box_per_image` is now adjusted automatically to the given training data. If this value is lower than the number of labeled particles per micrograph, not all particle are taken into account during training. The automatic adaptation ensures that it is high enough.

* Fixed a crash of crYOLO when no particles were picked. (Thanks to Grigory Sharov)

* Fixed network architectures "crYOLO" and "YOLO".


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

Version 1.4.10
***************
    * Add `coords2cbox` to `cryolo_boxmanager_tools.py`. Converts coords to CBOX.
    * Add `cbox2coords` to `cryolo_boxmanager_tools.py`. Converts CBOX to coords.
    * Fix bug that checkboxes are restored loaded correctly after loading them for folder of tomograms.
    * Fix bug box sizes are not changed proberly if multiple tomograms are loaded.

Version 1.4.6
*************

* Add script `sphericalprior` to `cryolo_boxmanager_tools.py`. Calculate priors of particles around a spherical model. It estimates 2 out of the 3 Euler angles a priori by calculating a vector from the centre of the sphere to each picked particle on the surface of the sphere. (Thanks to Andrea Nans)
* Fix problem that particles disappear when you change the micrograph

Version 1.4.5
*************

* Fix priors2star script when input star file does not contain required prior columns (Thanks to Liang Chen).

Version 1.4.4
*************

* Fix random crashes when saving tomo training data to disk (Thanks to Tom Dendooven)
* Fix problem that BM creates empty files for tomograms that where not selected.

Version 1.4.3
*************

* Fix problem that multiple box sets were not shown in different colors
* Fix problem when displaying boxes on tomograms with different sizes.

Version 1.4.2
*************

* Fixes several bugs, including that low pass filtering disappeared in certain cases.

Version 1.4.1
*************

* Fixes a bug, that after filaments are resized (or box distance changed) and then safed to disk, the new size/box size is not applied.

Version 1.4.0
*************

* The boxmanager now support tomograms.
* Added the option to pick filaments in micrographs and slices of tomograms.
* Minor redesign of the GUI
* cryolo_boxmanager_tools.py provide commands the prepare your tomo picking for further processing.
    * :option:`scale`: Allows to you scale any coordiantes file that crYOLO produces
    * :option:`coords2warp`: Prepares a star file that can be directly used in warp (M)
    * :option:`priors2star`: Add filament prior information to particle.star from relion.
* Many internal changes

All these changes were mainly implemented by Luca Lusnig. Thanks Luca :-)

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
