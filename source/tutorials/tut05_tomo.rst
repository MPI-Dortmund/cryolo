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
* crYOLO 1.8 comes with several library updates:
    * Cuda 9 -> Cuda 10
    * Tensorflow 1.10.1 -> Tensorflow 1.15.4
    * NumPy 1.14.5 -> NumPy 1.18.5
* CBOX files are now written in the STAR format.

As it is still a beta, I recommend to keep your old crYOLO installation and
install it in a fresh environment. Here are the steps:

>>> conda create -n cryolo_3d -c anaconda pyqt=5 python=3.6 cudatoolkit=10.0.130 cudnn=7.6.5 numpy==1.18.5 libtiff wxPython=4.0.4

Activate the environment:

>>> source activate cryolo_3d

Install crYOLO 1.8 Beta 1

>>> pip install 'cryolo[gpu]'==1.8.0b1

2. Data preparation
^^^^^^^^^^^^^^^^^^^

The boxmanager now has basic support for tomograms but it's still under heavy development.

Start it with:

>>> cryolo_boxmanager.py

Open your reconstructed tomogram (.rec/.mrc) with:

File -> Open -> Tomogram -> File

Now place your boxes on a couple of slices (e.g. 10).

Choose the boxsize according the largest dimension of your target structure.

After you did that press

File -> Save

It will save it in three formats (EMAN, CBOX, STAR). We will need the CBOX
files for training.


3. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

4. Configuration
^^^^^^^^^^^^^^^^
Choose the action :guilabel:`config`. The configuration is basically the same as for picking particles from scratch.
Set the :guilabel:`boxsize` to the value you've choosen when creating the training data. Set folder
where your tomogram is as :guilabel:`train_image_folder` and the CBOX folder that you was created in step
2 as :guilabel:`train_annot_folder`.

If you binned (4x/8x) the tomograms, please choose a lower absolute threshold for
the low pass filter. In the tab :guilabel:`Denoising options` you need to set :guilabel:`low_pass_cutoff`
to e.g 0.2 or 0.3.

Press :guilabel:`Start` to create the configuration file.

5. Training
^^^^^^^^^^^
A
