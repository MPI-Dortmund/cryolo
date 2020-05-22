crYOLO integration into the module system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This page shortly descripes how to integrate crYOLO into you module system. I assume you followed the normal installation instructions. The first you have to do is to find out your environment path with:

>>> conda env list

In my case it is:

 cryolo                  /home/twagner/Applications/miniconda3/envs/cryolo

The next step is to navigate to your module files and create a folder for crYOLO. Inside this folder, create file with the current version number like “1.5”. The content of this file looks like this:

.. code-block::

    #%Module -*- tcl -*-
    ##
    ## dot modulefile
    ##
    proc ModulesHelp { } {

      puts stderr "\tAdds anaconda to your environment variables,"
    }

    module-whatis "Adds anaconda to your environment variables"


    set              root              /home/twagner/Applications/miniconda3/envs/cryolo

    setenv CRYOLOPATH $root
    prepend-path      PATH              $root/bin