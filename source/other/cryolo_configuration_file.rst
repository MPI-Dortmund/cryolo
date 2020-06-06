crYOLO configuration file
^^^^^^^^^^^^^^^^^^^^^^^^^
The config file is organized in the sections model, training, validation and other. This is how a typical
configuration file looks like:

.. code-block:: JSON

    {
        "model": {
            "architecture": "PhosaurusNet",
            "input_size": 1024,
            "anchors": [
                220,
                220
            ],
            "max_box_per_image": 700,
            "norm": "STANDARD",
            "filter": [
                0.1,
                "filtered_tmp/"
            ]
        },
        "train": {
            "train_image_folder": "train_image/",
            "train_annot_folder": "train_annot/",
            "train_times": 10,
            "pretrained_weights": "",
            "batch_size": 6,
            "learning_rate": 0.0001,
            "nb_epoch": 200,
            "object_scale": 5.0,
            "no_object_scale": 1.0,
            "coord_scale": 1.0,
            "class_scale": 1.0,
            "saved_weights_name": "out/mymodel.h5",
            "debug": true
        },
        "valid": {
            "valid_image_folder": "",
            "valid_annot_folder": "",
            "valid_times": 1
        },
        "other": {
            "log_path": "logs/"
        }
    }


In the following you find a description of each entry.

Model section
*************
* :samp:`architecture`: The network used in the backend of crYOLO. Right we support :guilabel:`crYOLO`, :guilabel:`YOLO`, :guilabel:`PhosaurusNet`. Default and recommended is :guilabel:`PhosaurusNet`.

* :samp:`input_size`: This is the size to which the short side of your input image is rescaled before passed through the network (it is NOT the size of your micrograph!). The long side will be scaled according the aspect ratio. In this example, a square image would be resized to 1024x1024.

* :samp:`anchors`: Anchors in YOLO are kind of a priori knowledge. You should specifiy your box size here.

* :samp:`max_box_per_image`:  Maximum number of particles in the image. Only for handling the memory. Keep the default of 700.

* :samp:`norm`: Normalization that is applied to the images. :guilabel:`STANDARD` will subtract the image mean and divide by the standard deviation. Experimental: Gaussian Mixture Models (:guilabel:`GMM`) fit a 2 component GMM to you image data and normalizes according the brighter component. This ensures that it always normalize with respect to ice but slows down the training.

* :samp:`overlap_patches`: Optional and deprecated. Only needed when using patch mode. Specifies how much the patches overlap. In our lab, we always keep the default value.

* :samp:`num_patches`: Optional and deprecated. If specified the patch mode will be used. A value of “2” means, that 2×2 patches will be used. With PhosaurusNet you typically don't need it.

* :samp:`filter`: Optional. Specifies the absolute cut-off frequency for the low-pass filter and the corresponding output folder. CrYOLO will automatically filter the data in :file:`train_image_folder` and :file:`valid_image_folder` and save it into the output folder. It will automatically check if a image provided in the train_image_folder is already filtered and use it in case. Otherwise it will filter it. :ref:`You can also use neural network based filtering<denoise-janni-label>`.

Training section
****************
* :samp:`train_image_folder`: Path to the image folder containing the images to train on. This could either be a separated folder containing ONLY your training data, but it could also be just the directory containing all of your images. CrYOLO will try to find the image based on annotation data you provided in :file:`train_annot_folder`.

* :samp:`train_annot_folder`: Path to folder containing the your annotation files like box or star files. Based on the filename crYOLO will try to find the corresponding images in :file:`train_image_folder`. It will search for image files, which containing the box filename.

* :samp:`train_times`: How often each image is presented to the network during one epoch. Default is 10 and should be kept until you have many training images.

* :samp:`pretrained_weights`: Path to h5 file that is used for initialization. Until you want to use weights from a previous dataset as initialization, the filename specified here should be same as saved_weights_name.

* :samp:`batch_size`: Specified the number of images crYOLO process in parallel during training. Strongly depending on the memory of your graphic card. 4 should be fine for GPUs with 8GB memory. You can increase in case you have more memory or decrease if you have memory problems. Bigger batches tend to improve convergence and even the final error.

* :samp:`learning_rate`: Defines the step size during training. Default should be kept.

* :samp:`nb_epoch`: Maximum number of epochs the network will train. I basically never reach this number, as crYOLO stops training if it recognize that the validation loss is not improving anymore.

* :samp:`object_scale`: Penalty scaling factor for missing picking particles.

* :samp:`no_object_scale`: Penalty scaling factor for picking background.

* :samp:`coord_scale`: Penalty scaling factor for errors in estimating the correct position.

* :samp:`class_scale`: Irrelevant, as crYOLO only has the “class” “particle”.

* :samp:`log_path`: Path to folder. During training, crYOLO saves there some logs for visualization in tensorboard. Tensorboard is used to visualize curves for training and validation loss.

* :samp:`saved_weights_name`: Every time the network improves in terms of validation loss, it will save the model into the file specified here.

* :samp:`debug`: If true, the network will provide several statistics during training.

Validation section
******************

* :samp:`valid_image_folder`: If not specified, crYOLO will simply select 20% of the training data for validation. However it is possible to specify to use specific images for validation. This should be the path to folder containing these files.

* :samp:`valid_annot_folder`: If not specified, crYOLO will simply select 20% of the training data for validation. However it is possible to specify to use specific images for validation. This should be the path to folder containing these validation box files.

* :samp:`valid_times`: How often each image is presented the network during validation. 1 should be kept.
