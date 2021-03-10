In the GUI you have to fill in the mandatory fields:

.. image:: ../img/cryolo_training_202003.png
    :width: 600

The default number of :guilabel:`warmup` :abbr:`epochs (One epoch is a complete pass through the training data.)` is fine as long as you don't want to refine an existing model.
During the warmup training epochs it will not try to estimate the size of your particle, which helps
crYOLO to converge.

.. admonition:: When does crYOLO stop the training?

    When you start the training, it will stop when the “loss” metric on the validation data does not
    improve 10 times in a row. This is typically enough. In case you want to give the training more
    time to find the best model you can increase the “not changed in a row” parameter to a higher value by
    setting the :guilabel:`early` argument in the :guilabel:`Optional arguments` to, for example, 15.