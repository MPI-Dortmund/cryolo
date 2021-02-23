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
the centre of the filaments but only parts of it. Moreover, crYOLO often picks in empty parts of tomogram (buffer).
You can easily add a couple of those empty slices to your training set by activating the checkbox in front of it.

After you did that press

:guilabel:`File` -> :guilabel:`Save`

crYOLO will ask you for your desired inter-box distance than it will save the training data as CBOX to disk.

3. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

4. Configuration
^^^^^^^^^^^^^^^^

next

