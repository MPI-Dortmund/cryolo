Import coordinates into Relion 4
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this tutorial we describe how to import crYOLO coordinates into Relion 4. I assume that your micrographs are somewhere in the Relion project directory.

1. Move or softlink your coordinates somewhere into the relion project directory.

2. Change directory into the relion project directory

 .. prompt:: bash $

    cd path/to/my/relion/project/directory/

3. Create the :file:`autopick.star` file with the following command:

 .. prompt:: bash $

    cryolo_boxmanager_tools.py createAutopick -m path/to/micrographs/*.mrc -c path/to/box/or/star/files/*.star -o output/

 .. warning::

    Its important to use relatives paths for the :guilabel:`-m` and :guilabel:`-c` option.


 It will create the :file:`autopick.star` file in the folder :file:`output`.

4. Open Relion and select the :guilabel:`Particle extraction` Job. For :guilabel:`micrograph STAR file`, select the :file:`micrographs.star` file from your CTF estimation. For :guilabel:`Input coordinates` choose the freshly generated :file:`autopick.star`.

Now your particles should get extracted!