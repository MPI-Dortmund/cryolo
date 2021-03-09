You might want to run a low pass filter before you start picking. Switch to tab :guilabel:`Filtering` and press :guilabel:`Apply`
to get a low pass filtered version of your currently selected image. An absolute
frequency cut-off of 0.1. The allowed values are 0 - 0.5. Lower values means stronger filtering.

.. image:: ../img/cryolo_boxmanager_filament_example_202103.png
    :width: 300
    :align: left

Here is how to pick filaments:

* Place a filament box: Click with :kbd:`LMB` + Hold at the start of the filament. Then drag the mouse to the position where the filament box should end and release the :kbd:`LMB`.
* Remove filament box: Hold :kbd:`Control` pressed and click with the :kbd:`LMB` inside the box you want to remove.

You can change the box width in the main window, by changing the number in the text field :guilabel:`Box size`. Press :guilabel:`Set` to apply it to all picked filaments. For training crYOLO, you should the use a box width ~2x bigger than
your filament width.

If you have images that do not contain particles but only contamination / ice you can add them to your training set by activate the checkbox in front of the image.