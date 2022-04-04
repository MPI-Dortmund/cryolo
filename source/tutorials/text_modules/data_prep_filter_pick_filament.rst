Before you can start picking filaments, you need to change :guilabel:`Picking` from :guilabel:`Particle` to :guilabel:`Filament`. Then you can pick filaments as follows:

* Place a filament box: Click with :kbd:`LMB` at the start of the filament and then drag the mouse to the position where the filament box should end and release the :kbd:`LMB`
* Remove filament box: Hold :kbd:`Control` pressed and click with the :kbd:`LMB` inside the box you want to remove.

You can change the box width in the main window, by changing the number in the text field :guilabel:`Box size`. Press :guilabel:`Set` to apply it to all picked filaments. For training crYOLO, you should the use a box width ~2x bigger than
your filament width.

If you have images that do not contain filaments but only contamination / ice you can add them to your training set by activate the checkbox in front of the image.
