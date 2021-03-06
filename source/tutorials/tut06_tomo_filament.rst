Tutorial 6: Pick filaments in tomograms (BETA)
==============================================

.. warning::

    UNDER CONSTRUCTION!!!!

This tutorial explains how to pick filaments in a tomogram. Therefore you need to label
a couple of slices manually and train cryolo as you always did.

.. warning::

    Please be aware that picking tomograms with crYOLO is still in beta state.
    Therefore this tutorial is targeted at rather advanced users and does not contain
    excessive details.

1. Installation
^^^^^^^^^^^^^^^
Please see tutorial 5 for installation instructions.

2. Data preparation
^^^^^^^^^^^^^^^^^^^
The boxmanager now has basic support for tomograms, but it's still under heavy development.

>>> cryolo_boxmanager.py

Open your reconstructed tomogram (.rec/.mrc) with:

:guilabel:`File` -> :guilabel:`Open` -> :guilabel:`Tomogram` -> :guilabel:`File`

Change the picking from :guilabel:`Particle` to :guilabel:`Filament`. Choose a :guilabel:`box size`
which is roughly 2x-3x the width of your filament.

Label your filaments in some slices (e.g. 10). Label it even if the slices does not show
the centre of the filaments but only parts of it. Do do that click on the place where the filament starts
and hold the mouse button and move to the position of the box. Curvy filaments should to be splitted
into multiple straight filaments.

Moreover, crYOLO often picks in empty parts of tomogram (buffer). You can easily add a couple of
those empty slices to your training set by activating the checkbox in front of those slices.

After you did that press

:guilabel:`File` -> :guilabel:`Save`

crYOLO will ask you for your desired inter-box distance than it will save the training data as CBOX to disk.

3. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

4. Configuration
^^^^^^^^^^^^^^^^

Choose the action :guilabel:`config`. The configuration is basically the same as for picking particles from scratch.
Set the :guilabel:`boxsize` to the value you've choosen when creating the training data. Set folder
where your tomogram is as :guilabel:`train_image_folder` and the CBOX folder that you created in step
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
Choose the action :guilabel:`predict`. You now need to make changes in three tapbs.

In the :guilabel:`Required arguments` tabs you need to choose
your configuration file from step 2 in field :guilabel:`conf`. For the field :guilabel:`weights` you choose the .h5
that you got after step 5. In :guilabel:`input` you choose the folder which contain the tomograms you want to pick.

In the :guilabel:`Filament options` you simply need to activate the checkbox :guilabel:`filament`. Als

.. admonition:: directional_method

    To trace the filaments in each slice of the tomogram, the local direction of the filament has to be estimated.
    There are two methods available: :guilabel:`PREDICTED` and :guilabel:`CONVOLUTION`