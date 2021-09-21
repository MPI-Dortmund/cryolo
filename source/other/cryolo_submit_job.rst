.. _queueing-label:

Submit crYOLO to queueing system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you use crYOLO on a cluster, you might want to submit to the queue. While for :guilabel:`config` this is not necessary and can be done on the headnode, the :guilabel:`train` and :guilabel:`predict`  action is computational expensive and you might want to submit it to a GPU node.

Therefore you need to prepare a submission template, that crYOLO can use to fill in the specific command. The following placeholders will replaced by crYOLO:


* :samp:`XXX_SXCMD_LINE_XXX`: This placeholder will be replaced with the respective crYOLO command.
* :samp:`XXX_SXMPI_NPROC_XXX`: Right now, crYOLO does not support running on multiple nodes. Therefore placeholder will be replaced by 1.
* :samp:`XXX_SXMPI_JOB_NAME_XXX`: The will be replaced with "CRYOLO"

A vanilla submission template can be look like this:
