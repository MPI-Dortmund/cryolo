Troubleshooting
===============

crYOLO is very slow and does not use the GPUs to the full potential
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

So far, this problem was only `reported <https://listserv.gwdg.de/pipermail/sphire/2022-August/001001.html>`_ for CentOS7 but other distributions might be affected as well.

The problem was related to problematic entries in the ``LD_LIBRARY_PATH``. Please check if there are many entries in your path:

.. prompt:: bash $

    echo $LD_LIBRARY_PATH

Especially CUDA entries from other software packages are a problem. If the ``LD_LIBRARY_PATH`` is not empty, try if the following prefix fixes your problem. In front of the ``cryolo_train.py`` / ``cryolo_predict.py`` command you put ``LD_LIBRARY_PATH=''``, e.g.:

.. prompt:: bash $

    LD_LIBRARY_PATH='' cryolo_train.py -c your_config -w 5

If that is working you can make the fix permanent. The following instructions will set your ``LD_LIBRARY_PATH`` to an empty value when activating the cryolo environment. It will restore the old ``LD_LIBRARY_PATH``, once the environment gets deactivated. I assume that your cryolo environment is called ``cryolo``. Do the following:

.. prompt:: bash $

    conda activate cryolo
    conda env config vars set LD_LIBRARY_PATH=
    conda deactivate cryolo
    conda activate cryolo

If you now run

.. prompt:: bash $

    echo $LD_LIBRARY_PATH

it should be empty. If you do

.. prompt:: bash $

    conda deactivate

and run

.. prompt:: bash $

    echo $LD_LIBRARY_PATH

you should see all your libraries again.

Now, the fix should be permanent and you don't need to add ``LD_LIBRARY_PATH=''`` in front of the crYOLO commands.

.. _cryolo-glibc-label:
crYOLO crashed with glibc errors
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::
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