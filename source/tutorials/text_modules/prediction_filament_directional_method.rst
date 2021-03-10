.. admonition:: directional_method

    To trace the filaments in 2D, the local direction of the filament has to be estimated.
    There are two methods available:

        * With :guilabel:`PREDICTED` you use the predicted direction learned by crYOLO. **This is the recommended method**.

        * With :guilabel:`CONVOLUTION` an elliposid mask with the width given by :guilabel:`filament_width` is rotated and convolved with the input image. The direction with the highest response gives the local direction of the filament. This method is mainly for backwards compatibility with earlier crYOLO versions (< 1.8).