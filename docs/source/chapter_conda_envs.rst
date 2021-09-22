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

Exporting an Anaconda environment
---------------------------------