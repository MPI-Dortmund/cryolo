Troubleshooting
===============

.. _cryolo-freeze-label:

crYOLO freezes
^^^^^^^^^^^^^^

On some machines crYOLO freezes during or at the end of training. The problem comes together with
multiprocessing and is deeply in one of the libraries we use. You can solve it by using
multithreading instead of multiprocessing. There for you can either use the option :option:`--use_multithreading`
or make it a permanent change by changing the environment variables in your crYOLO enviroment:

>>> conda activate cryolo
>>> conda env config vars set CRYOLO_MP_START="fork"
>>> conda env config vars set CRYOLO_USE_MULTITHREADING="True"

You need to reactivate your enviroment to make the changes wokring by

>>> conda activate cryolo

Now you use multithreading insteast of multiprocessing.


crYOLO has memory problems
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

crYOLO can crash during training because of memory problems.
In those cases you can try the following:

* Reduce the :guilabel:`batch_size`. I recommend to reduce it by 1 stepwise. I would not choose a value below 3. You find the :guilabel:`batch_size` in your configuration file or in the :guilabel:`Training options` tab of the :guilabel:`config` Action
* Reduce the :guilabel:`input_size`. Instead of 1024 you can choose any multiple of 32. Therefore 31*32=992 would next smaller input size. Don't go too low (< 768) as you might become problem with very small particles. You find the  :guilabel:`input_size` in your configuration file or in the :guilabel:`Model options` tab of the :guilabel:`config` Action.

I need more help
^^^^^^^^^^^^^^^^

Find help at our `mailing list <https://listserv.gwdg.de/mailman/listinfo/sphire>`_!