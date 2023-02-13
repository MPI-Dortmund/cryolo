Train crYOLO based on good classes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Here I assume you that you have star file :file:`particles.star` that contain the columns :guilabel:`_rlnMicrographName`, :guilabel:`_rlnCoordinateX` and :guilabel:`_rlnCoordinateY`.
When you are working with RELION, you should have such a file.

.. hint::
    **cryoSPARC**

    Support for cryoSPARC .cs files will follow soon!

With the cryolo boxmanager 1.9 you can create training data based on this star file.

To create the training annotation based on good 2D classes your need to extract star files per micrograph. You can do that with the following command:

.. prompt:: bash $

 cryolo_boxmanager_tools.py class2Dextract -s particles.star -o out_annotation/ -n 10

It is better to train crYOLO with micrographs where many particles are annotated. The parameter :guilabel:`-n 10` writes those 10 star files that contain the most particles.

Next you can simply follow the :ref:`standard workflow to train crYOLO <tutorial-2-label>`, but instead creating manual training data (step 1) you use the generated STAR files.



