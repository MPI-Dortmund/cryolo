Troubleshooting
===============

crYOLO freezes
^^^^^^^^^^^^^^

On some machines crYOLO freezes during or at the end of training. The problem comes together with
multiprocessing and is deeply in one of the libraries we use. You can solve it by using
multithreading instead of multiprocessing. There for you can either use the option --use_multithreading
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
In those cases crYOLO you can try the following:

* Reduce the batchsize. I recommend to reduce it by 1 stepwise. I would not choose a value below 3.
* Reduce the input image size. Try 768 instead of 1024. It should work, as along your particles are not really small.

I need more help
^^^^^^^^^^^^^^^^

Find help at our `mailing list <https://listserv.gwdg.de/mailman/listinfo/sphire>`_!