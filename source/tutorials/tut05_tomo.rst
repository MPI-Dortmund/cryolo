Tutorial 5: Pick particles in tomograms
=======================================

This tutorial explains how to pick particles in a tomogram. Therefore you need to label
a couple of slices manually and train cryolo.

If you followed the installation instructions, you now have to activate the cryolo virtual environment with

.. prompt:: bash $

    source activate cryolo

1. Data preparation
^^^^^^^^^^^^^^^^^^^

.. include:: data_preparation_tomo.rst

2. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

3. Configuration
^^^^^^^^^^^^^^^^
Choose the action :guilabel:`config`. The configuration is basically the same as for picking particles from scratch.
Set the :guilabel:`boxsize` to the value you've choosen when creating the training data. Set folder
where your tomogram is as :guilabel:`train_image_folder` and the CBOX folder that you created in step
2 as :guilabel:`train_annot_folder`.

If you binned (4x/8x) the tomograms, please choose a lower absolute threshold for
the low pass filter. In the tab :guilabel:`Denoising options` you need to set :guilabel:`low_pass_cutoff`
to e.g 0.3 or 0.4.

Press :guilabel:`Start` to create the configuration file.

4. Training
^^^^^^^^^^^
.. include:: training.rst


5. Prediction
^^^^^^^^^^^^^
Select the action :guilabel:`predict` and fill in the data for the :guilabel:`Required arguments` tab.
Next select the :guilabel:`Tomography options` tab. Activate the checkbox :guilabel:`Activate tomograghy picking mode`.
Keep the other values default. See the info box about information about the tomography picking mode.

.. note::

    **Tomography picking**

    When using the tomography picking mode, crYOLO will first pick your target structure on each slice
    separately and then trace it through the volume. There are three parameters that be used to adjust
    this tracing: :guilabel:`tracing_search_range`, :guilabel:`tracing_memory` and :guilabel:`tracing_min_length`.
    However, you can safely use the default values. Only change the parameters if you encounter problems.

    Two picked boxes in separate slices are considered to belong to the same particle when they are
    within the :guilabel:`tracing_search_range` and the gab between the two slices is not bigger than the value configured
    in :guilabel:`tracing_memory`. Traces that contain less boxes then the the value configured in
    :guilabel:`tracing_min_length` are considered as false positive and are removed.

Now you can press :guilabel:`Start`.

crYOLO will write four folders into the :file:`output` directory. :file:`CBOX_3D`, :file:`CBOX_UNTRACED`, :file:`EMAN_3D` and :file:`coords`.

The folder :file:`CBOX_3D` contains 3D boxes in the CBOX format. The folder :file:`CBOX_UNTRACED`
contains the picks for each slices that were used for tracing. Ignore it for now, it will be relevant
in a later boxmanager version. The folder :file:`EMAN_3D` contains the coordinates of 3d boxes in EMAN2 format.
The files in :file:`coords` contain files that can directly used in IMOD.


6. Visualization
^^^^^^^^^^^^^^^^
You can visualize the 3D boxes using the napari boxmanager. The command is:

.. prompt:: bash $

    napari_boxmanager /path/to/your/tomogram.mrc path/to/CBOX_3D/tomogram.cbox






