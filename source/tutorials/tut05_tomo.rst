Tutorial 5: Pick particles in tomograms (BETA)
=======================================

This tutorial explains how to pick particles in a tomogram. Therefore you need to label
a couple of slices manually and train cryolo as you always did.

.. warning::

    Please be aware that picking tomograms with crYOLO is still in beta state.
    Therefore this tutorial is targeted at rather advanced users and does not contain
    excessive details.

1. Installation
^^^^^^^^^^^^^^^
You need to install crYOLO 1.8 (BETA) for this. Some things changed with crYOLO 1.8:

* crYOLO 1.8 adds a picking mode for tomography
* crYOLO 1.8 comes with several library updates. This is part of the ongoing transition to tensorflow 2:
    * Cuda 9 -> Cuda 10
    * Tensorflow 1.10.1 -> Tensorflow 1.15.4
    * NumPy 1.14.5 -> NumPy 1.18.5
    * Keras 2.2.5 -> Keras 2.3.1
* CBOX files are now written in the STAR format.
* Now crYOLO allows to use .star files as input. crYOLO will pick all micrographs in the column '_rlnMicrographName'.
As the path in this column is relative to your project directory, you need to star crYOLO from your project directory.
* The boxmanager can now supports tomograms.

As it is still a beta, I recommend to keep your old crYOLO installation and
install crYOLO 1.8 in a fresh environment. Here are the steps:

>>> conda create -n cryolo_3d -c anaconda pyqt=5 python=3.7 cudatoolkit=10.0.130 cudnn=7.6.5 numpy==1.18.5 libtiff wxPython=4.0.4

Activate the environment:

>>> source activate cryolo_3d

Install the crYOLO 1.8 beta:

>>> pip install 'cryolo[gpu]'==1.8.0b10

2. Data preparation
^^^^^^^^^^^^^^^^^^^

The boxmanager now has basic support for tomograms, but it's still under heavy development.
We will improve the user experience as quick as possible.

Start it with:

>>> cryolo_boxmanager.py

Open your reconstructed tomogram (.rec/.mrc) with:

:guilabel:`File` -> :guilabel:`Open` -> :guilabel:`Tomogram` -> :guilabel:`File`

Choose the box size according the largest dimension of your target structure.

Label your target structure in some slices (e.g. 10). Label it even if the slices does not show
the centre of the structure but only parts of it. Moreover, crYOLO often picks in empty parts of tomogram (buffer).
You can easily add a couple of those empty slices to your training set by activating the checkbox in front of it.

After you did that press

:guilabel:`File` -> :guilabel:`Save`

It will save it in three formats (EMAN, CBOX, STAR). We will need the **CBOX**
files for training.

3. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

4. Configuration
^^^^^^^^^^^^^^^^
Choose the action :guilabel:`config`. The configuration is basically the same as for picking particles from scratch.
Set the :guilabel:`boxsize` to the value you've choosen when creating the training data. Set folder
where your tomogram is as :guilabel:`train_image_folder` and the CBOX folder that you has created in step
2 as :guilabel:`train_annot_folder`.

If you binned (4x/8x) the tomograms, please choose a lower absolute threshold for
the low pass filter. In the tab :guilabel:`Denoising options` you need to set :guilabel:`low_pass_cutoff`
to e.g 0.3 or 0.4.

Press :guilabel:`Start` to create the configuration file.

5. Training
^^^^^^^^^^^
.. include:: training.rst


6. Prediction
^^^^^^^^^^^^^
Select the action :guilabel:`predict` and fill in the data for the :guilabel:`Required arguments` tab.
Next select the :guilabel:`Tomography options` tab. Activate the checkbox :guilabel:`Activate tomograghy picking mode`.
Keep the other values default. See the info box about information about the tomography picking mode.

.. note::

    **Tomography picking**

    When using the tomography picking mode, crYOLO will first pick your target structure on each slice
    separately and then trace it through the volume. There are three options to adjust this tracing:

    Two picked boxes in separate slices are considered to belong to the same particle when they are
    within the :guilabel:`tracing_search_range` and the slices are not further apart than value configured
    in :guilabel:`tracing_memory`. Traces that contain less boxes then the the value configured in
    :guilabel:`tracing_min_length` are considered as false positive and are removed.

Now you can press :guilabel:`Start`.

crYOLO will write four folders into the :file:`output` directory. :file:`CBOX_3D`, :file:`CBOX_UNTRACED`, :file:`EMAN_3D` and :file:`coords`.

The folder :file:`CBOX_3D` contains 3D boxes in the CBOX format. The folder :file:`CBOX_UNTRACED`
contains the picks for each slices that were used for tracing. Ignore it for now, it will be relevant
in a later boxmanager version. The folder :file:`EMAN_3D` contains the coordinates of 3d boxes in EMAN2 format.
The files in :file:`coords` contain files that can directly used in IMOD.


7. Visualization
^^^^^^^^^^^^^^^^
The current state of the boxmanager can't visualize the 3D boxes properly. We recommend to use
e2spt_boxer.py from EMAN2 to visualize the file in :file:`EMAN_3D`.

>>> e2spt_boxer.py /path/to/your/tomogram.mrc

Then click File -> Read Box Coord




