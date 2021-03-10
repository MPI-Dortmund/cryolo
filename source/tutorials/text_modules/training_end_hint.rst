The final model will be written to disk as specified in saved_weights_name in your configuration file.

Now press the :guilabel:`Start` button to start the training.

.. hint::

    **Alternative: Train crYOLO using the command line**

    To run the training on GPU 0 with 5 warmup-epochs and an early stop of 15 navigate to the folder with :file:`config_cryolo.json` file, :file:`train_image` folder etc.

    .. prompt:: bash $

    cryolo_train.py -c config_cryolo.json -w 5 -g 0 -e 15

    The final model file will be written to disk.