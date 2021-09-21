.. _queueing-label:

Submit crYOLO to queueing system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you use crYOLO on a cluster, you might want to submit to the queue. While for :guilabel:`config` this is not necessary and can be done on the headnode, the :guilabel:`train` and :guilabel:`predict`  action is computational expensive and you might want to submit it to a GPU node.

Therefore you need to prepare a submission template, that crYOLO can use to fill in the specific command. A vanilla submission template for Slurm can be look like this (your probably need to adapt the partition):

.. code-block:: bash

        #!/bin/bash
        #SBATCH --partition gpunodes1
        #SBATCH --ntasks XXX_SXMPI_NPROC_XXX
        #SBATCH --ntasks-per-node 20
        #SBATCH --cpus-per-task 1
        #SBATCH --job-name XXX_SXMPI_JOB_NAME_XXX

        XXX_SXCMD_LINE_XXX

The placeholders (XXX_PLACEHOLDER_XXX) in the submission template will replaced by crYOLO:

* :samp:`XXX_SXCMD_LINE_XXX`: This placeholder will be replaced with the respective crYOLO command.
* :samp:`XXX_SXMPI_NPROC_XXX`: Right now, crYOLO does not support running on multiple nodes. Therefore placeholder will be replaced by 1.
* :samp:`XXX_SXMPI_JOB_NAME_XXX`: The will be replaced with "crYOLO"


When you press the :guilabel:`Submit` button you are asked for the path of the :guilabel:`Submission template` and the :guilabel:`submission command` of your queueing system. You can add additional arguments to the :guilabel:`submission command` if necessary.

It is possible to set default values for the :guilabel:`Submission template` and the :guilabel:`submission command`. To do that you can set the following enviroment variables:

* :samp:`CRYOLO_SUBMIT_CMD`: Default value for the submission command
* :samp:`CRYOLO_SUBMIT_SCRIPT`: Default value for the path to the submission script





