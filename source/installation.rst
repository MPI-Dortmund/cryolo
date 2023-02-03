Installation and Download
=========================

System requirements
^^^^^^^^^^^^^^^^^^^

At our institute in Dortmund, crYOLO is running on the following operation systems:

* Ubuntu 18.04 LTS
* Ubuntu 20.04
* CentOS 7

We don't test it but it should run on Windows as well.

Moreover the following GPUs are used:

* NVIDIA Titan V
* NVIDIA GTX 1080
* NVIDIA GTX 1080Ti
* NVIDIA RTX 2080 TI
* NVIDIA GV 100

As the GPU accelerated version of tensorflow does not support MacOS, crYOLO does not support it either.

crYOLO depends on CUDA Toolkit and the cuDNN library. These will be automatically installed
during crYOLO installation.


Install crYOLO
^^^^^^^^^^^^^^

.. note::

    Please read the :doc:`Complimentary Science Software License <other/license>` before using crYOLO. If you are interested in using crYOLO in a commercial context please contact stefan.raunser@mpi-dortmund.mpg.de

The following instructions assume that pip and anaconda or miniconda are available. In case you
have a old cryolo environment installed, first remove the old one with:

.. prompt:: bash $

    conda env remove --name cryolo

With CUDA 11
""""""""""""

The official tensorflow 1.15x does not support CUDA 11. To get support for it, we need use a custom tensorflow version from nvidia. The following steps do explain how setup crYOLO with this custom nvidia.

The first step is to create a new virtual environment:

.. prompt:: bash $

    conda create -n cryolo -c conda-forge -c anaconda pyqt=5 python=3 numpy==1.18.5 libtiff wxPython=4.1.1  adwaita-icon-theme 'setuptools<66'

Activate the environment:

.. prompt:: bash $

    conda activate cryolo

Next you need to install a package that allows the installation of a custom tensorflow version from nvidia:

.. prompt:: bash $

    pip install nvidia-pyindex

To install crYOLO with CUDA 11 support you need to run:

.. prompt:: bash $

    pip install 'cryolo[c11]'

With CUDA 10
""""""""""""

.. warning::
    Recent NVIDIA graphic cards (e.g. RTX30XX, A5000)  do not longer support CUDA 10. Only use the CUDA 10 setup if your have specific reason for it.

The first step is to create a new virtual environment:

.. prompt:: bash $

    conda create -n cryolo -c conda-forge -c anaconda pyqt=5 python=3.7 cudatoolkit=10.0.130 cudnn=7.6.5 numpy==1.18.5 libtiff wxPython=4.1.1  adwaita-icon-theme

Activate the environment:

.. prompt:: bash $

    conda activate cryolo

In case you run **crYOLO on a GPU** run:

.. prompt:: bash $

    pip install 'cryolo[gpu]'

But if you want to run crYOLO on a CPU run:

.. prompt:: bash $

    pip install 'cryolo[cpu]'


.. warning::
    In case you run into glibc errors, you can find a solution in our :ref:`troubleshooting section <cryolo-glibc-label>`

.. hint::
    You can also integrate crYOLO as :ref:`Environment Module <cryolo-module-label>`

**That's it!**

You might want to check if everything is running as expected. Here is a reference example:

:doc:`Reference example with TcdA1 <other/ref_example>`

.. _general-model-label:

Download the general models
^^^^^^^^^^^^^^^^^^^^^^^^^^^

We provide three general models. One for cryo-EM images which was trained on low-pass filtered images,
another one for cryo-EM images but trained for images denoised by JANNI and one for negative stain images.

For cryo images (low-pass filtered)
""""""""""""""""""""""""""""""""""

:Datasets: 43 real, 10 simulated, 10 particle free datasets on various grids with contamination

:Uploaded: 27 May 2020

:Download: `ftp <ftp://ftp.gwdg.de/pub/misc/sphire/crYOLO-GENERAL-MODELS/gmodel_phosnet_202005_N63_c17.h5>`_ `https <https://owncloud.gwdg.de/index.php/s/AdVdYdcCg4XaNRw>`_

:Config: :ref:`Commands to create the config file can be found here <config-general-model>`.

For cryo images (neural network denoised with JANNI)
""""""""""""""""""""""""""""""""""""""""""""""""""""

:Datasets: 43 real, 10 simulated, 10 particle free data sets on various grids with contamination

:Uploaded: 27 May 2020

:Download: `ftp <ftp://ftp.gwdg.de/pub/misc/sphire/crYOLO-GENERAL-MODELS/gmodel_phosnet_202005_nn_N63_c17.h5>`_ `https <https://owncloud.gwdg.de/index.php/s/RVEnx1t0t7DTbgA>`_

:Config: :ref:`Commands to create the config file can be found here <config-general-model>`.

For negative stain images
"""""""""""""""""""""""""

:Datasets: 10 real data sets

:Uploaded: 26 February 2019

:Download: `ftp <ftp://ftp.gwdg.de/pub/misc/sphire/crYOLO-GENERAL-MODELS/gmodel_phosnet_negstain_20190226.h5>`_ `https <https://owncloud.gwdg.de/index.php/s/KpSw1gGIM3Q3KGa>`_

:Config: :ref:`Commands to create the config file can be found here <config-general-model>`.
