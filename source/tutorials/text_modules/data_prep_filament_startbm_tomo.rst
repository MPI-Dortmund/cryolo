In the following I will assume that your tomo data is in the folder :file:`images`.

The next step is to create training data. To do so, we have to pick a few slices manually of your tomogram. Ideally, each slices is picked to completion. :ref:`However, it is not necessary to pick all particles. crYOLO will still converge if you miss some (or even many)<sparse-picking-label>`. If you want to reach generalization across multiple tomograms, we recommend to include multiple tomograms in your tomogram.

.. admonition:: How many slices have to be picked?

    It depends! Typically 10 slices are a good start. However, that number may increase / decrease
    due to several factors:

        * A very heterogeneous tomogram could make it necessary to pick more slices.
        * When you refine a general model, you might need to pick fewer images.
        * If your slices are only sparsely decorated, you may need to pick more images.

    We recommend that you start with 10 slices, then autopick your data, check the results and
    finally decide whether to add more slices to your training set.

To create your training data, we developed a dedicated napari plugin called “napari-boxmanager”.

Start the box manager with the following command:

.. prompt:: bash $

    napari_boxmanager
