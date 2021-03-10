.. include:: text_modules/data_prep_filament_startbm.rst

For opening your tomograms you have to options:

    * **Single tomogram**: Press :guilabel:`File` -> :guilabel:`Open` -> :guilabel:`Tomogram` -> :guilabel:`File` and the select one file from the :file:`images` directory. The first slice of the tomogram should pop up. You can navigate in the directory tree through the slices.
    * **Folder**: Press :guilabel:`File` -> :guilabel:`Open` -> :guilabel:`Tomogram` -> :guilabel:`Folder` and the select the :file:`images` directory. The first slice of the first tomogram fill pop up. You can find all tomograms organized in subtrees.

.. include:: text_modules/data_prep_filter.rst

.. include:: text_modules/data_prep_filter_pick_particles.rst

Label your particles in some slices ideally on multiple tomograms. Label them even if the slices do not show
the centre of the particle but only slice of it.

After you did that press

:guilabel:`File` -> :guilabel:`Save`

It will create three subdirectories, each for one format (:file:`EMAN`, :file:`CBOX`, :file:`STAR`). We will need the :file:`CBOX` files for training.

Create a new folder called :file:`train_annot` and copy the files from :file:`CBOX` into this folder. Alternatively you can also directly specify the :file:`CBOX` folder during the configuration step.


