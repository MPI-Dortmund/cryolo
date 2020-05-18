Installation and Download
=========================

System requirements
^^^^^^^^^^^^^^^^^^^

crYOLO was tested on Ubuntu 16.04.4 LTS and Ubuntu 18.04 with an NVIDIA Geforce 1080 / Geforce 1080Ti.

However, it should run on Windows as well.

As the GPU accelerated version of tensorflow does not support MacOS, crYOLO does not support it either.

crYOLO depends on CUDA Toolkit 9.0 and the cuDNN 7.1.2 library. These will be automatically installed
during crYOLO installation.


Install crYOLO
^^^^^^^^^^^^^^

.. note::

    Please read the :doc:`Complimentary Science Software License </license>` before using crYOLO. If you are interested in using crYOLO in a commercial context please contact stefan.raunser@mpi-dortmund.mpg.de

The following instructions assume that pip and anaconda or miniconda are available. In case you
have a old cryolo environment installed, first remove the old one with:

>>> conda env remove --name cryolo

After that, create a new virtual environment:

>>> conda create -n cryolo -c anaconda python=3.6 pyqt=5 cudnn=7.1.2 numpy==1.14.5 cython wxPython==4.0.4 intel-openmp==2019.4

Activate the environment:

>>> source activate cryolo

In case you run **crYOLO on a GPU** run:

>>> pip install 'cryolo[gpu]'

But if you want to run crYOLO on a CPU run:

>>> pip install 'cryolo[cpu]'

**That's it!**

You might want to check if everything is running as expected. Here is a reference example:

:doc:`Reference example with TcdA1 </ref_example>`

.. _general-model-label:

Download the general models
^^^^^^^^^^^^^^^^^^^^^^^^^^^

We provide three general models. One for cryo-EM images which was trained on low-pass filtered images,
another one for cryo-EM images but trained for images denoised by JANNI and one for negative stain images.

For cryo images (low-pass filtered)
""""""""""""""""""""""""""""""""""

:Datasets: 43 real, 10 simulated, 10 particle free datasets on various grids with contamination

:Uploaded: 16 March 2020

:Download: `Link <ftp://ftp.gwdg.de/pub/misc/sphire/crYOLO-GENERAL-MODELS/gmodel_phosnet_202002_N63.h5>`_

For cryo images (neural network denoised with JANNI)
""""""""""""""""""""""""""""""""""""""""""""""""""""

:Datasets: 43 real, 10 simulated, 10 particle free datasets on various grids with contamination

:Uploaded: 17 March 2020

:Download: `Link <ftp://ftp.gwdg.de/pub/misc/sphire/crYOLO-GENERAL-MODELS/gmodel_phosnet_202003_nn_N63.h5>`_

For negative stain images
"""""""""""""""""""""""""

:Datasets: 10 real datasets

:Uploaded: 26 February 2019

:Download: `Link <ftp://ftp.gwdg.de/pub/misc/sphire/crYOLO-GENERAL-MODELS/gmodel_phosnet_negstain_20190226.h5>`_
