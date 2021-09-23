How to use Conda environments
=============================

Anaconda is one of the largest commercial Python distributions. It has become a defacto standard for packaging Scientific Software and comes with it's own packaging system, conda. If you are developing software using Anaconda the preferred way of installing software is by using the conda command line tool. If the packages are available in Anaconda's package-repos they are tested to work together. It is often also not a good idea to mix conda and pip package-repositories.

In many situations it is often required to use both pip and conda packages. In these situations it is possible within Anaconda to create conda environments. These are derived from the builtin virtual environment tools in Python, but extended and made easier to use.

Creating a Anaconda environment
-------------------------------

An anaconda environment is created using the **conda create** command. As shown in the following example:

.. code-block:: bash

    $ conda create -n calfem-test
    Collecting package metadata (current_repodata.json): done
    Solving environment: done

    ## Package Plan ##

    environment location: e:\anaconda3\envs\calfem-test



    Proceed ([y]/n)? y

    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done
    #
    # To activate this environment, use
    #
    #     $ conda activate calfem-test
    #
    # To deactivate an active environment, use
    #
    #     $ conda deactivate   

This creates a new empty environment ready for use.

This environment doesn't come with a Python interpreter, so that has to be installed by using:

.. code-block:: bash

    $ conda install python

This will install the latest Python interpreter in the new environment

It is also possible to do this in a single command:

.. code-block:: bash

    $ conda create -n calfem-test python

It is also possible to specify more packages to install by adding them on the command line.

The version of Python or packages can be specified by using an equal sign and a version number as shown below:

.. code-block:: bash

    $ conda create -n calfem-test python=3.8

This will create an environment with the latest version of Python 3.8.


Activating and deactivating Anaconda environments
-------------------------------------------------

To use an environment it has be activated. Activating an environment is done using the **conda activate** command. The command takes name of the environment as an additional parameter. In the following example we activate the previously created environment:

.. code-block:: bash

    $ conda activate calfem-test

    (calfem-test) $

This will setup all the search paths for the environment. The prompt is also modified to indicate which environment is activate.

Please note that creating an empty environment does not come with an Python interpreter by default.

An environment can be deactivate by using the **conda deactivate** command:

.. code-block:: bash

    (calfem-test) $ conda deactivate
    (base) $

No environment name is required for deactivating an environment. 

Removing an environment
-----------------------

Removing a created environment is done using the **conda env remove** command:

conda env remove -n calfem-test

Remove all packages in environment e:\anaconda3\envs\calfem-test:

.. code-block:: bash

    (base) $ conda remove -n calfem-test

    Remove all packages in environment e:\anaconda3\envs\calfem-test:
    ...

Cloning an existing environment
-------------------------------

An exact copy of an existing environment can be created using the **--clone** option:

.. code-block:: bash

    (base) $ conda create -n calfem-dev-2 --clone calfem-test
    Source:      e:\anaconda3\envs\calfem-dev
    Destination: e:\anaconda3\envs\calfem-dev-2
    Packages: 158
    Files: 13121

Exporting an Anaconda environment
---------------------------------

You can list all packages and their versions using the **conda list** command:

.. code-block:: bash

    (base) $ conda list calfem-dev --explicit
    # This file may be used to create an environment using:
    # $ conda create --name <env> --file <this file>
    # platform: win-64
    @EXPLICIT
    https://repo.anaconda.com/pkgs/main/win-64/ca-certificates-2021.7.5-haa95532_1.conda
    https://repo.anaconda.com/pkgs/main/noarch/tzdata-2021a-h5d7bf9c_0.conda
    https://repo.anaconda.com/pkgs/main/noarch/pyopenssl-20.0.1-pyhd3eb1b0_1.conda
    ...
    https://repo.anaconda.com/pkgs/main/noarch/urllib3-1.26.6-pyhd3eb1b0_1.conda
    https://repo.anaconda.com/pkgs/main/noarch/requests-2.26.0-pyhd3eb1b0_0.conda
    https://repo.anaconda.com/pkgs/main/noarch/sphinx-4.0.2-pyhd3eb1b0_0.conda

This list can also be saved to a file that then can be used to recreate the environment.

.. code-block:: bash

    (base) $ conda list calfem-dev --explicit > spec-file.txt

Using this file it is now possible to create an environmnet with the exact set of packages.

.. code-block:: bash

    (base) conda create --name calfem-dev-3 --file spec-file.txt
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction:
    ...

Conda and pip
-------------

Pip can be used to install software in a conda environment. However, package information for Pip-packages are not exported using the **conda list** command. Pip-packages must be handled separately for example using the **pip freeze** command. 

If possible it is always better to use the packages that are available in the conda repositories instead of using packages from the pip-package repository.

Using conda environments in Jupyter Notebooks
---------------------------------------------

If you want to use a conda environment in a Jupyter Notebook it has to be added to the list of availble kernels. This can be done by using the **ipykernel** package. This package is installed with the following commands:

.. code-block:: bash

    (base) $ conda activate your-environment
    (your-environment) $ conda install ipykernel
    (base) $ conda deactivate

It is now possible to switch environment from within a jupyter-notebook started from the base-environment:

.. code-block:: bash

    (base) $ jupyter-notebook

It is now possible to create a new notebook using the environment by selecting **New / Python [conda env:your-environment]**



