Tutorial 3: Pick filaments using a model trained for your data
==============================================================
When picking filaments, it is important to identify each filament individually. This allows specific
spacing of the boxes (i.e., the helical rise) to maximize the number of particles. CrYOLO supports
this method of picking filaments.

Filament mode on actin:

.. list-table::

    * - .. figure:: ../img/action_tracing_2.png

      - .. figure:: ../img/actin_tracing_1.png


Filament mode on MAVS (EMPIAR-10031) :

.. list-table::

    * - .. figure:: ../img/filament_tracing_02.png

      - .. figure:: ../img/filament_tracing_03.png

1. Data preparation
^^^^^^^^^^^^^^^^^^^
.. include:: data_preparation_filament.rst

2. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

3. Configuration
^^^^^^^^^^^^^^^^
.. include:: configuration_filament.rst

4. Training
^^^^^^^^^^^
.. include:: training_filament.rst

5. Picking
^^^^^^^^^^
Select the action :guilabel:`predict` and fill all arguments in the :guilabel:`Required arguments` tab:

.. image:: ../img/cryolo_prediction_202003.png
    :width: 600

Now select the :guilabel:`Filament options` tab and check :guilabel:`Activate filament mode` and define the :guilabel:`box_distance` (e.g. 20 for 90% overlap when using a box size if 200).

.. include:: text_modules/prediction_filament_directional_method.rst

.. image:: ../img/cryolo_filament_202103.png
    :width: 600

The directory :file:`output_boxes` will be created and all results are saved there. The traced
filaments will be saved in the eman2 helix format with particle coordinates. But also EMAN2 and STAR start/end coordinates.

.. admonition:: Import into Relion

    You can find a detailed description :ref:`how to import crYOLO filament coordinates into Relion <import-filaments-label>` here.

Press the :guilabel:`Start` button to start the picking.

.. hint::

    **Evaluate directional estimates**

    You can check how well crYOLO did in estimating the directionality, which is crucial for the subsequent filament tracing.
    To do this for a given threshold (e.g. 0.3), you can run

    .. prompt:: bash $

        cryolo_boxmanager_tools.py cbox_directions -m full_data/  -c output_boxes/CBOX/ -t 0.3 -o output_boxes/directions/

    You will find a png plot for each micrograph in :file:`output_boxes/directions/` after the script has finished.

.. hint::

    **Alternative: Run prediction in command line**

    Let's assume you want to pick a filament, the box size
    is 200×200 and you want a 90% overlap (-bd 20). Moreover, you wish that each filament has at
    least 6 boxes (-mn 6). The micrographs are in the :file:`full_data` directory. Than the picking
    command would be:

    .. prompt:: bash $

        cryolo_predict.py -c config_cryolo.json -w cryolo_model.h5 -i full_data --filament -bd 20 -o boxes/ -g 0 -mn 6



6. Visualize the results
^^^^^^^^^^^^^^^^^^^^^^^^
.. include:: visualize_results.rst