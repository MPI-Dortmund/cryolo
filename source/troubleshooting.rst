Troubleshooting
===============

.. _cryolo-glibc-label:
crYOLO crashed with glibc errors
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. hint::
    This problem should be solved with crYOLO 1.8.2


**Alternative A:**

Within your crYOLO environment run:

.. prompt:: bash $

    pip install nvidia-tensorflow==1.5.5+nv22.1

**Alternative B:**

The current CUDA 11 instructions need a quite recent glibc version (>=2.29). Not all systems provide such a
recent version. However, you can manually compile it and force crYOLO to use it:

1. Download and compile a recent glibc (>= 2.29)

.. prompt:: bash $

    wget http://ftp.gnu.org/gnu/libc/glibc-2.34.tar.xz
    tar xvf glibc-2.34.tar.xz
    mkdir glibc-2.34/build
    cd glibc-2.34/build
    sudo mkdir /opt/glibc-2.34
    ../configure --prefix=/opt/glibc-2.34
    make -j 8
    sudo make install

2. Add environment variable for the cryolo environment ( I assume the environment name is "cryolo"):

.. prompt:: bash $

    conda activate cryolo
    conda env config vars set LD_PRELOAD=/opt/glibc-2.34/lib/libm.so.6

3. Reload your environment

.. prompt:: bash $

    conda deactivate
    conda activate cryolo

Now you should be able to run cryolo with CUDA 11.

Thanks to Wolfgang Lugmayr for the instructions!


.. _cryolo-freeze-label:
crYOLO freezes
^^^^^^^^^^^^^^

.. note::

    Since crYOLO 1.7.4 this problem is solved. Multithreading replaced multiprocessing.

On some machines crYOLO freezes during or at the end of training. The problem comes together with
multiprocessing and is deeply in one of the libraries we use. You can solve it by using
multithreading instead of multiprocessing. There for you can either use the option :option:`--use_multithreading`
or make it a permanent change by changing the environment variables in your crYOLO environment:

.. prompt:: bash $

    conda activate cryolo
    conda env config vars set CRYOLO_MP_START="fork"
    conda env config vars set CRYOLO_USE_MULTITHREADING="True"

You need to reactivate your environment to make the changes working by

.. prompt:: bash $

    conda activate cryolo

Now you use multithreading instead of multiprocessing.


crYOLO has memory problems
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

crYOLO can crash during training because of memory problems.
In those cases you can try the following:

* Reduce the :guilabel:`batch_size`. I recommend to reduce it by 1 stepwise. I would not choose a value below 3. You find the :guilabel:`batch_size` in your configuration file or in the :guilabel:`Training options` tab of the :guilabel:`config` Action
* Reduce the :guilabel:`input_size`. Instead of 1024 you can choose any multiple of 32. Therefore 31*32=992 would next smaller input size. Don't go too low (< 768) as you might become problem with very small particles. You find the  :guilabel:`input_size` in your configuration file or in the :guilabel:`Model options` tab of the :guilabel:`config` Action.

I need more help
^^^^^^^^^^^^^^^^

Find help at our `mailing list <https://listserv.gwdg.de/mailman/listinfo/sphire>`_!