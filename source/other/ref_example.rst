crYOLO reference example
^^^^^^^^^^^^^^^^^^^^^^^^

Here we provide quick run through example for training and picking with crYOLO. The main purpose is to check if your setup is running as expected. I will not provide detailed explanations in this text. Please note that there is a :ref:`detailed tutorial <tutorial-label>`.

Reference setup
***************

We run this example on a machine with the following specification:

* Titan V
* Intel Core i9 7920X @ 2.90 Ghz
* SSD Harddrive
* crYOLO 1.5.0

Download reference data and getting started
*******************************************

You can download the reference data (TcdA1) here:

`Link to reference example <https://owncloud.gwdg.de/index.php/s/SjzATaIMZaANrnm>`_

Then unzip the data:

>>> unzip toxin_reference.zip -d toxin_reference/
>>> cd toxin_reference

The :file:`toxin_reference` directory contains multiple folders / files:

* :file:`train_image`: Folder with 12 training images
* :file:`train_annot`: Folder with 12 box files for the training images
* :file:`config_phosnet.json`: Configuration file for crYOLO
* :file:`reference_model.h5`: Model that I've trained on my machine using the commands below.
* :file:`reference_results`: Picked particles using my machine and the reference model.

Before you start training / picking please activate your environment:

>>> source activate cryolo

Training
********

The training is done with this command:

>>> cryolo_train.py -c config_phosnet.json -w 5 -e 5 -g 0

crYOLO needs 5 minutes 50 seconds to converge (5 warmup + 10 “normal” epochs). The best validation loss was 0.03042. These numbers might be a little bit different on your case.

Prediction
**********

>>> cryolo_predict.py -c config_phosnet.json -w model.h5 -i unseen_examples/ -o my_results

It picked 1617 particles on 12 micrographs in 3 seconds. Including filtering the image and loading the model the command needed 38 seconds.


Visualize results
*****************

>>> cryolo_boxmanager.py -i unseen_examples/ -b my_results/CBOX/

