Here is how to pick particles:

First you need to create a new layer for picking particles. Switch to the tab :guilabel:`Organize_layer` and click :guilabel:`Create particle layer`. I assume that your only have one image stack open, in case you don't please adapt :guilabel:`Target image layer` accordingly. Switch to the :guilabel:`boxmanager` tab. Open the list of coordinates by pressing the little :guilabel:`+` as shown below.

.. figure:: ../img/napari/boxmanager_table_plus.png
    :width: 150

The basic usage of the boxmanager is as follows:

* Place a box: Press key :kbd:`2` to switch the layer control (left side) to the plus symbol. Then you can place a box with :kbd:`LMB`.
* Move a box: Press key :kbd:`3` to switch the layer control to selection symbol. Then you can drag a box by holding :kbd:`LMB`.
* Delete a box: Press key :kbd:`3` to switch the layer control to selection symbol.  Click on a box and press :kbd:`DEL`
* Zoom: You can use your mouse wheel to zoom in and out.

You can change the box size in the main window, by changing the number in the text field :guilabel:`boxsize` and confirm with pressing :kbd:`Enter`.
For picking, you should the use minimum sized square which encloses your particle.

If you have images that do not contain particles but only contamination / ice you can add them to your training set by activate the checkbox in front of the image.