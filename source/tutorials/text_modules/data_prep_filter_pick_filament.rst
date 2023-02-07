Here is how to pick filaments:

First you need to create a new layer for picking filaments. Switch to the tab :guilabel:`Organize_layer` and click :guilabel:`Create filament layer`. I assume that you only have one image stack open, in case you don't please adapt :guilabel:`Target image layer` accordingly. Switch to the :guilabel:`boxmanager` tab. Open the list of coordinates by pressing the little :guilabel:`+`.

The filaments are placed as follows:

.. |pth| image:: ../img/napari/path_icon.png
.. |arrow| image:: ../img/napari/shape_arrow_icon.png

* Place a filament: Switch to layer control to |pth|. Click with :kbd:`LMB` at the start of the filament. You can click along the filament. Double click to end the picking of filament.
* Remove filament: Switch to layer control to |arrow| (shortcut key :kbd:`5`). Click on your filament and press :kbd:`DEL`.

You can change the box width in the main window, by changing the number in the text field :guilabel:`boxsize`. Press :guilabel:`Enter` to apply it to all picked filaments. For training crYOLO, you should the use a box width ~2x bigger than
your filament width.

If you have images that do not contain filaments but only contamination / ice you can add them to your training set by activate the checkbox in front of the image.
