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
.. include:: data_preparation_filament_tomo.rst

3. Start crYOLO
^^^^^^^^^^^^^^^
.. include:: start_cryolo.rst

4. Configuration
^^^^^^^^^^^^^^^^

Choose the action :guilabel:`config`. The configuration is basically the same as for picking particles from scratch.
Set the :guilabel:`boxsize` to the value you've choosen when creating the training data. Choose the folder
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
Choose the action :guilabel:`predict`. You now need to make changes in three tabs:

In the :guilabel:`Required arguments` tabs you need to choose
your configuration file from step 2 in field :guilabel:`conf`. For the field :guilabel:`weights` you choose the .h5
that you got after step 5. In :guilabel:`input` you choose the folder which contain the tomograms you want to pick.

In the :guilabel:`Filament options` you simply need to activate the checkbox :guilabel:`filament`.  The default
parameters for the other options should be ok for most cases.

.. admonition:: directional_method

    To trace the filaments in each slice of the tomogram, the local direction of the filament has to be estimated.
    There are two methods available: :guilabel:`PREDICTED` and :guilabel:`CONVOLUTION`.

    With :guilabel:`PREDICTED` you use the predicted direction learned by crYOLO. This is the recommended method.

    With :guilabel:`CONVOLUTION` an elliposid mask with the width given by :guilabel:`filament_width` is rotated
    and convolved with the input image. The direction with the highest response gives the local direction of the
    filament. This method is mainly for backwards compatibility with earlier crYOLO versions (< 1.8).

In the :guilabel:`Tomography options` tab also simply activate the checkbox :guilabel:`tomogram`. The default
parameters for the other options should be ok for most cases.

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

7. Visualize the results
^^^^^^^^^^^^^^^^^^^^^^^^
You can open all files (except :file:`COORDS_TRACED` and :file:`DISTR`) within the cryolo boxmanager. Just type

>>> cryolo_boxmanager.py

to start the boxamanger.

8. Troubleshooting
^^^^^^^^^^^^^^^^^^

What to do when the 3D tracing didnt produced good results? Many filaments were not traced at all?
Here we give some recommendations what to do.

1. Check the particles picked by crYOLO by loading the folder :file:`CBOX` in the boxmanager. You should see multiple particles picked on every filament.
    - If not, do they appear when you lower the confidence threshold?
    - In case this doesnt help you should add more training data. Especially those slices where crYOLO missed a lot of particles of picked in the background. Then you retrain and start the prediction again.
    - (Coming soon with a boxmanager update) Check if the directional estimations are pointing in along the filament.


2. Check the filament tracing: Open the :file:`CBOX_FILAMENTS_UNTRACED` folder in the boxmanager. You should see you filaments traced.
    - If not, have you checked if the directional estimations in step 1? In case case they were not accurate, you might want to try to retrain your model again, but this time, give the your training a little mit more time by setting the :guilabel:`early` argument within the :guilabel:`train` action in the :guilabel:`Optional arguments` to, for example, 20. It also helps if you add more training data.
    - If you observed that boxes withhin one filament in step 1 were not overlapping, you can try to increase the search range. Open the action :guilabel:`predict`, open the :guilabel:`Filament options` tab and increase the :guilabel:`search_range_factor` from 1.41 to, for example, 2.

3. Check the 3D tracing settings:
    - Are you filaments very tilted in your dataset? It might help to decrease the :guilabel:`tracing_min_edge_weight` within the :guilabel:`Tomography options` of the action :guilabel:`Predict`
    - If you filaments where nicely traced in 2D but many are missing in 3D it might be that multiple seperate filaments where merged into one filament. This leads to effect that the averages filament is somewhere in the tomogram, but is not following any filament. On option to resolve this issue is to increse the :guilabel:`tracing_min_edge_weight`.

4. Nothing helped? Find help at our `mailing list <https://listserv.gwdg.de/mailman/listinfo/sphire>`_!