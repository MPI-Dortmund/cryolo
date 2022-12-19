.. include:: text_modules/data_prep_filament_startbm.rst

.. image:: ../img/napari/boxmanager_filament_uncollapse.png
    :width: 300
    :align: left

Now press :guilabel:`File` -> :guilabel:`Open Folder...` and the select the :file:`images` directory.

.. include:: text_modules/data_prep_filter.rst

.. include:: text_modules/data_prep_filter_pick_filament.rst

.. image:: ../img/boxmanager_filament_example.png
    :width: 600
    :align: center

.. include:: text_modules/data_prep_filament_save.rst

In the folder :file:`boxes` you just created, you will find three subdirectories:

* :file:`CBOX_FILAMENT`: Contains filament coordinates segmented (according :guilabel:`box distance`) into several boxes in the cbox format
* :file:`EMAN_HELICON`: Contains filament coordinates segmented into several boxes in eman helicon format.
* :file:`EMAN_START_END`: Contains filament coordinates specified by start and end coordinates in EMAN format.
* :file:`STAR_START_END`: Contains filament coordinates specified by start and end coordinates in STAR (Relion) format.

In principle you can use any format for training, in this tutorial we will use :file:`CBOX_FILAMENT`. Create a new folder called :file:`train_annot` and copy the files from :file:`CBOX_FILAMENT` into this folder. Alternatively you can also directly
specify the :file:`CBOX_FILAMENT` folder during the configuration step.


