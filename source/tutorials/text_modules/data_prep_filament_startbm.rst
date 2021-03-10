In the following I will assume that your image data is in the folder :file:`images`.

The next step is to create training data. To do so, we have to pick single filaments manually in several images. Ideally, the images are picked to completion. :ref:`However, it is not necessary to pick all particles. crYOLO will still converge if you miss some (or even many)<sparse-picking-label>`.

.. admonition:: How many images have to be picked?

    It depends! Typically 10 images are a good start. However, that number may increase / decrease
    due to several factors:

        * A very heterogeneous background could make it necessary to pick more images.
        * When you refine a general model, you might need to pick fewer images.
        * If your micrograph is only sparsely decorated, you may need to pick more images.

    We recommend that you start with 10 images, then autopick your data, check the results and
    finally decide whether to add more micrographs to your training set. If you refine a general
    model, even 5 images might be enough.

.. image:: ../img/cryolo_boxmanager_filament_202103.png
    :width: 300
    :align: left

To create your training data, crYOLO is shipped with a tool called “boxmanager”.

Start the box manager with the following command:

.. prompt:: bash $

    cryolo_boxmanager.py