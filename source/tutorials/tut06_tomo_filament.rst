Tutorial 6: Pick filaments in tomograms
=======================================

This tutorial explains how to pick filaments in a tomogram. Therefore you need to label
a couple of slices manually and train cryolo.

If you followed the installation instructions, you now have to activate the cryolo virtual environment with

.. prompt:: bash $

    source activate cryolo

1. Data preparation
^^^^^^^^^^^^^^^^^^^
.. include:: data_preparation_filament_tomo.rst

2. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

3. Configuration
^^^^^^^^^^^^^^^^

Choose the action :guilabel:`config`. The configuration is basically the same as for picking particles from scratch.
Set the :guilabel:`boxsize` to the value you've choosen when creating the training data. Choose the folder
where your tomogram is as :guilabel:`train_image_folder` and the CBOX folder that you created in step
2 as :guilabel:`train_annot_folder`.

If you binned (4x/8x) the tomograms, please choose a lower absolute threshold for
the low pass filter. In the tab :guilabel:`Denoising options` you need to set :guilabel:`low_pass_cutoff`
to e.g 0.3 or 0.4.

Press :guilabel:`Start` to create the configuration file.

4. Training
^^^^^^^^^^^
.. include:: training_filament.rst

5. Prediction
^^^^^^^^^^^^^
Choose the action :guilabel:`predict`. You now need to make changes in three tabs:

* In the :guilabel:`Required arguments` tabs you need to choose your configuration file from step 2 in field :guilabel:`conf`. For the field :guilabel:`weights` you choose the .h5 that you got after step 5. In :guilabel:`input` you choose the folder which contain the tomograms you want to pick.

* In the :guilabel:`Filament options` you simply need to activate the checkbox :guilabel:`filament`.  The default parameters for the other options should be ok for most cases.

.. include:: text_modules/prediction_filament_directional_method.rst

* In the :guilabel:`Tomography options` tab also simply activate the checkbox :guilabel:`tomogram`. The default parameters for the other options should be ok for most cases.

.. admonition:: 3D Filament tracing

    To trace your filaments in 3D, the filaments are first traced slicewise and then grouped together
    across slices using graphs. The connected components in that graph are groups of 2D filaments that
    represent a single 3D filament. Those 2D filaments are then averages to create a 3D filament.

Now press the :guilabel:`Start` button to start the pick your tomogram. The output will be various folders:

* :file:`CBOX_FILAMENTS_TRACED`:  Filaments traced in 3D.
* :file:`CBOX_FILAMENTS_UNTRACED`: Filaments traced in 2D but not in 3D. This is the internal input for 3D tracing and mainly for troubleshooting.
* :file:`CBOX`: Particles picked by crYOLO. This is the input for the 2D filament tracing and mainly for troubleshooting (see section 8).
* :file:`COORDS_TRACED`: 3D filament coordinates as they are needed for visualization in imod.
* :file:`DISTR`: Contains size distribution information. Not informative in this case. Only helpful with a general model, which does not yet exist for filaments.

6. Visualize the results
^^^^^^^^^^^^^^^^^^^^^^^^
You can open all files (except :file:`COORDS_TRACED` and :file:`DISTR`) within the cryolo boxmanager. Just type

.. prompt:: bash $

    cryolo_boxmanager.py

to start the boxamanger.

7. Extraction
^^^^^^^^^^^^^
Once you are happy with the results, you need to prepare everything for further processing

Option 1: Use the files from COORDS_TRACED as inputs for relion particle extraction
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

RELION sub-volume averaging requires a certain file structure in the project directory, for example:

::

    ~/RelionProjectDir/Tomograms/tomogram1/tomogram1.mrc
    ~/RelionProjectDir/Tomograms/tomogram1/tomogram1.mrcs (aligned stack)
    ~/RelionProjectDir/Tomograms/tomogram1/tomogram1.order
    ~/RelionProjectDir/Tomograms/tomogram1/tomogram1.tlt
    ~/RelionProjectDir/Tomograms/tomogram1/tomogram1.coords

The :file:`tomogram1.coords` file requires the 3D coordinates per tomogram of your particle positions. The files from :file:`COORDS_TRACED` (those without the _fid postfix) can be used directly at this point.

However, the files need to get rescaled in case binned tomograms were used for picking. Lets assume you picked on 4x binned tomograms. Then you can rescale the .coords files with:

.. prompt:: bash $

    cryolo_boxmanager_tools.py prior2star -i /path/to/COORDS_TRACED -o /path/to/COORDS_TRACED_RESCALED -s 4.0

The rescaled .coords files can then be used for sub-volume averaging.

In order to incorporate priors from the filament data into the star file using this strategy, this information needs to be extracted from the :file:`*_fid.coords` files and added to the :file:`particles.star` output from the extraction job in RELION. The following command generates an augmented .star file based on the RELION-generated :file:`particle.star` file:

.. prompt:: bash $

    cryolo_boxmanager_tools.py priors2star -i particles.star -fi /path/to/COORDS_TRACED_RESCALED -o .

This command will add extract the filament information from the files in :file:`COORDS_TRACED_RESCALED` and add them to the information from the :file:`particles.star` file and write the augmented file :file:`particles_with_priors.star` to disk. This can then be used for subsequent subtomogram averaging.



Option 2: Convert the files from COORDS_TRACED into star for input into Warp
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

If using tomograms produced by Warp to pick particles using cryolo, one can use the particle coordinates to produce a star file that is compatible with the particle extraction functionality within Warp.

.. prompt:: bash $

    cryolo_boxmanager_tools.py coords2warp -i /path/to/COORDS_TRACED/ -o out_warp/ --scale 1.0 --apix PIXEL_SIZE --mag 10000

Dependend of your binning and microscope settings, you need to adapt the scale (:option:`--scale`), pixelsize (:option:`--apix`) and magnification (:option:`--mag`). You will find the Warp compatible star file in :file:`out_warp`.

7. Troubleshooting
^^^^^^^^^^^^^^^^^^

What to do when the 3D tracing didnt produced good results? Many filaments were not traced at all?
Here we give some recommendations what to do.

1. Check the particles picked by crYOLO by loading the folder :file:`CBOX` in the boxmanager. You should see multiple particles picked on every filament.
    - If not, do they appear when you lower the confidence threshold?
    - In case this doesnt help you should add more training data. Especially those slices where crYOLO missed a lot of particles of picked in the background. Then you retrain and start the prediction again.
    - (Coming soon with a boxmanager update) Check if the directional estimations are pointing in along the filament.


2. Check the filament tracing: Open the :file:`CBOX_FILAMENTS_UNTRACED` folder in the boxmanager. You should see you filaments traced.
    - If not, have you checked if the directional estimations in step 1? In case they were not accurate, you might want to try to retrain your model again, but this time, give the your training a little bit more time by setting the :guilabel:`early` argument within the :guilabel:`train` action in the :guilabel:`Optional arguments` to, for example, 20. It also helps if you add more training data.
    - If you observed that boxes within one filament in step 1 were not overlapping, you can try to increase the search range. Open the action :guilabel:`predict`, open the :guilabel:`Filament options` tab and increase the :guilabel:`search_range_factor` from 1.41 to, for example, 2.

3. Everything from the above to steps looks fine? Maybe you need to adjust the 3D tracing settings:
    - Are your filaments very tilted in your dataset? It might help to decrease the :guilabel:`tracing_min_edge_weight` within the :guilabel:`Tomography options` of the action :guilabel:`Predict`
    - If your filaments where nicely traced in 2D but many are missing in 3D it might be that multiple separate filaments where merged into one filament. This leads to effect that the averages filament is somewhere in the tomogram, but is not following any filament. One option to resolve this issue is to increase the :guilabel:`tracing_min_edge_weight`.

4. Nothing helped? Find help at our `mailing list <https://listserv.gwdg.de/mailman/listinfo/sphire>`_!