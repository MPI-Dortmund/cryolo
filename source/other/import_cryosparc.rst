Import coordinates into cryoSPARC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

crYOLO 1.9 writes STAR files that can be imported into cryoSPARC. Here is how you do it.

1. Create a new job :guilabel:`Import Particle Stack`

2. Choose as :guilabel:`Particle meta path` the :file:`cryosparc.star` file written by crYOLO.

3. In case you didn't pick on micrographs  that were motion corrected by cryoSPARC, plase activate :guilabel:`Remove leading UID in input micrograph path`

4. Activate :guilabel:`Ignore raw data`

5. Activate :guilabel:`Ignore pose data`

6. Click in :guilabel:`Queue Job`. You will see some warnings but can ignore them, its mostly that the STAR files do not include

 .. image:: ../img/cryosparc_output.png
    :width: 300
    :align: left


