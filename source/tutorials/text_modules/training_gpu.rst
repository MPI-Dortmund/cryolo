Now you are ready to train the model. In case you have multiple GPUs, you should first select a free
GPU. The following command will show the status of all GPUs:

.. prompt:: bash $

nvidia-smi

For this tutorial, we assume that you have either a single GPU or want to use GPU 0.

.. admonition:: Use a different or multiple GPUs

    In the :guilabel:`Optional arguments` tab you can change the GPU that should be used by crYOLO.
    If you have multiple GPUs (e.g. nvidia-smi lists GPU 0 and GPU 1) you can also use both by
    setting the GPU argument to '0 1'.