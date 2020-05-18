Select the action :guilabel:`predict` and fill all arguments in the :guilabel:`Required arguments` tab:

.. image:: ../img/cryolo_prediction_202003.png
    :width: 600

.. admonition:: Adjusting confidence threshold

    In crYOLO, all particles have an assigned confidence value. By default, all particles with a
    confidence value below 0.3 are discarded. If you want to pick less or more conservatively you might
    want to change this confidence threshold to a less (e.g. 0.2) or more (e.g. 0.4) conservative value
    in the :guilabel:`Optional arguments` tab. However, it is much easier to select the best threshold after
    picking using the CBOX files written by crYOLO as described in the next section.

.. admonition:: Monitor mode

    When this option is activated, crYOLO will monitor your input folder. This especially useful
    for automation purposes. You can stop the monitor mode by writing an empty file with the
    name :file:`stop.cryolo` in the input directory. Just add :option:`--monitor` in the command line or check
    the monitor box in in the :guilabel:`Optional arguments` tab

Press the the :guilabel:`Start` button to run the prediction.

After picking is done, you can find four folders in your specified output folder:

* :file:`CBOX`: Contains a coordinate file in .cbox format each input micrograph. It contains all detected particles, even those with a confidence lower the selected confidence threshold. Additionally it contains the confidence and the estimated diameter for each particle. Importing those files into the boxmanager allows you advanced filtering e.g. according size or confidence.

* :file:`EMAN`: Contains a coordinate file in .box format each input micrograph. Only particles with the an confidence higher then the selected (default: 0.3) are contained in those files.

* :file:`STAR`: Contains a coordinate file in .star format each input micrograph. Only particles with the an confidence higher then the selected (default: 0.3) are contained in those files.

* :file:`DISTR`: Contains the plots of confidence- and size-distribution. Moroever, it contains a machine readable text-file the summary statistics about these distributions and their raw data in separate text-files.

.. hint::

    **Alternative: Run prediction from the command line**

    To pick all your images in the directory :file:`full_data` with the model weight file :file:`cryolo_model.h5` (e.g. or :file:`gmodel_phosnet_X_Y.h5` when using the general model) and and a confidence threshold of 0.3 run:

    >>> cryolo_predict.py -c config.json -w cryolo_model.h5 -i full_data/ -g 0 -o boxfiles/ -t 0.3

    You will find the picked particles in the directory :file:`boxfiles`.