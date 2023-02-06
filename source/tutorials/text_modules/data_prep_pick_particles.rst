Start picking particles
"""""""""""""""""""""""

First you need to create a new layer for picking particles
 1. Switch to the tab :guilabel:`Organize_layer`.
 2. Click :guilabel:`Create particle layer`. I assume that you only have one image stack open, in case you don't please adapt :guilabel:`Target image layer` accordingly.
 3. Switch to the :guilabel:`boxmanager` tab. Open the list of coordinates by pressing the little :guilabel:`+`. You can now navigate in the image tree and start picking.

.. |plus| image:: ../img/napari/plus_icon.png
.. |arrow| image:: ../img/napari/shape_arrow_icon.png

The basic usage of the boxmanager is as follows:

* Place a box: Switch the layer control (left side) to |plus| (shortcut key :kbd:`2`). Then you can place a box with :kbd:`LMB`.
* Move a box: Switch  to the layer control to |arrow| (shortcut key :kbd:`3`). Then you can drag a box by holding :kbd:`LMB`.
* Delete a box: Switch the layer control to |arrow|.  Click on a box and press :kbd:`DEL`
* Zoom: You can use your mouse wheel to zoom in and out.

You can change the box size in the main window, by changing the number in the text field :guilabel:`boxsize` and confirm with pressing :kbd:`Enter`.
For picking, you should the use minimum sized square which encloses your particle.

If you have images that do not contain particles but only contamination / ice you can add them to your training set by activate the checkbox in front of the image.