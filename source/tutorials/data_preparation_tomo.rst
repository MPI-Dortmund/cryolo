.. include:: text_modules/data_prep_filament_startbm.rst

For opening your tomograms you have to options:

    * **Single tomogram**: Press :guilabel:`File` -> :guilabel:`Open File(s)...` and the select one file from the :file:`images` directory.
    * **Folder**: Press :guilabel:`File` -> :guilabel:`Open Folder...` and the select the :file:`images` directory.

.. image:: ../img/cryolo_bm_tomo_folder_202103.png
    :width: 300
    :align: left

.. include:: text_modules/data_prep_filter.rst

.. include:: text_modules/data_prep_pick_particles.rst

Label your particles in some slices ideally on multiple tomograms. Label them even if the slices do not show
the centre of the particle but only slice of it.

After you did that press

:guilabel:`File` -> :guilabel:`Save`

It will create three subdirectories, each for one format (:file:`EMAN`, :file:`CBOX`, :file:`STAR`). We will need the :file:`CBOX` files for training.

Create a new folder called :file:`train_annot` and copy the files from :file:`CBOX` into this folder. Alternatively you can also directly specify the :file:`CBOX` folder during the configuration step.


