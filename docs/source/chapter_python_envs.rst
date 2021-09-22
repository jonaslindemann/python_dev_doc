How to use Python virtual environments
======================================

Python can use a lot of third-party extension modules. Complex projects can depend on a lot of these modules. Installing extension modules in the base installation can work for smaller projects and scripts. However different projects can have dependencies on different incompatible libraries, making it hard to maintain an single Python installation for multiple projects. To solve this, Python contains a mechanism for creating isolated python environments that each can have it's own set of extension modules installed. This mechanics is called virtual environments and is accessible through the venv module.

A virtual environment contains is a copy of the Python base installation with only the absolute minimum of installed packages.

Using a virtual enviromnents (venv)
-------------------------------------

Creating a virtual environment is done by the following command:

.. code-block:: bash
    
    python -m venv myenv

This will create a separate directory, myenv, where copy of the Python base install will be installed. To use this new environment it has to be activated. This is done by calling/sourcing a special script that is available inside the environment, activate. On a windows installation a virtual environment is activated by calling:

.. code-block:: bash

    myenv\Scripts\activate.bat

On a Linux/Mac environment the environment is activated by sourcing:

.. code-block:: bash

    source myenv/bin/activate

When an enviromnent is activte the prompt is changed to display the currently active environment:

.. code-block:: bash

    (myenv) (base) E:\Users\Jonas\Development\python_dev_doc\examples>

If the environment is no longer used it can be deactivated using the following commands:

.. code-block:: bash

    myenv\Scripts\deactivate.bat

On a Linux/Mac environment the environment is activated by sourcing:

.. code-block:: bash

    source myenv/bin/deactivate

Managing packages in virtual environment
----------------------------------------

Packages or third-party packages in a virtual environment are installed and managed using the **pip** command just like in a base Python installation. The difference is that a newly created environment only holds just the required packages for the environment to work. 

Available packages in an enviromnent can be listed using the **pip list** command:

.. code-block:: bash

    $ pip list

Running this command in a base installation will produce a long list of packages:

.. code-block:: bash

    $ pip list
    Package                            Version
    ---------------------------------- -------------------
    alabaster                          0.7.12
    anaconda-client                    1.7.2
    anaconda-navigator                 2.0.3
    anaconda-project                   0.9.1
    anyio                              2.2.0
    appdirs                            1.4.4
    argh                               0.26.2
    argon2-cffi                        20.1.0
    asn1crypto                         1.4.0
    astroid                            2.5
    astropy                            4.2.1
    async-generator                    1.10
    atomicwrites                       1.4.0
    attrs                              20.3.0
    autopep8                           1.5.6
    Babel                              2.9.0

Doing the same thing in a newly created environment produces the following output.

.. code-block:: bash

    pip list
    Package    Version
    ---------- -------
    pip        20.2.3
    setuptools 49.2.1

Using virtualenv to create environments
---------------------------------------

**virtualenv** is a tool that provides additional options and also makes it easier to create virtual environments. This should preferable be installed in the base Python installation. The tool is installed using the following command:

.. code-block:: bash

    pip install virtualenv

Creating reproducable environments
----------------------------------

In many scientific workflows it is important to create reproducable workflows. This also extends to scientific software. Virtual environments are excellent to create a reproducable set of dependencies for a scientific workflow. 

When a environment has been created and packages have been installed, it is possible to create a list of required packages that can be used to recreate the excact environment. Using the **pip freeze** command it is possible to create a list of requirements that can be used as input in a **pip install** command. 

In the following example we will create a *requirements.txt* file containing the needed modules in the myenv environment. Listing the installed packages produces the following output:

.. code-block:: bash

    $ pip list
    Package           Version
    ----------------- -------
    calfem-python     3.5.10
    cycler            0.10.0
    gmsh              4.8.4
    kiwisolver        1.3.1
    matplotlib        3.4.3
    numpy             1.21.2
    Pillow            8.3.1
    pip               21.2.4
    PyOpenGL          3.1.5
    pyparsing         2.4.7
    PyQt5             5.15.4
    PyQt5-Qt5         5.15.2
    PyQt5-sip         12.9.0
    PyQtWebEngine     5.15.4
    PyQtWebEngine-Qt5 5.15.2
    python-dateutil   2.8.2
    PyVTK             0.5.18
    scipy             1.7.1
    setuptools        49.2.1
    six               1.16.0
    visvis            1.13.0
    wheel             0.37.0

Using the **pip freeze** command we can create a list of requirements.

.. code-block:: bash

    $ pip freeze > requirements.txt
    $ cat requirements.txt
    calfem-python==3.5.10
    cycler==0.10.0
    gmsh==4.8.4
    kiwisolver==1.3.1
    matplotlib==3.4.3
    numpy==1.21.2
    Pillow==8.3.1
    PyOpenGL==3.1.5
    pyparsing==2.4.7
    PyQt5==5.15.4
    PyQt5-Qt5==5.15.2
    PyQt5-sip==12.9.0
    PyQtWebEngine==5.15.4
    PyQtWebEngine-Qt5==5.15.2
    python-dateutil==2.8.2
    PyVTK==0.5.18
    scipy==1.7.1
    six==1.16.0
    visvis==1.13.0     

On Windows use **type requirements.txt**.

Using this file it is now possible to recreate a new environment using the following commands:

.. code-block:: bash

    $ python -m venv newenv
    $ newenv/Scripts/activate.bat
    (newenv) $ pip install -r myenv\requirements.txt
    Collecting calfem-python==3.5.10
    Using cached calfem_python-3.5.10-py3-none-any.whl (70 kB)
    Collecting cycler==0.10.0
    ...
    Successfully installed Pillow-8.3.1 PyOpenGL-3.1.5 PyQt5-5.15.4 PyQt5-Qt5-5.15.2 PyQt5-sip-12.9.0 PyQtWebEngine-5.15.4 PyQtWebEngine-Qt5-5.15.2 PyVTK-0.5.18 calfem-python-3.5.10 cycler-0.10.0 gmsh-4.8.4 kiwisolver-1.3.1 matplotlib-3.4.3 numpy-1.21.2 pyparsing-2.4.7 python-dateutil-2.8.2 scipy-1.7.1 six-1.16.0 visvis-1.13.0

If we activate and list the packages we should get the same packages in **newenv** as in **myenv**.

.. code-block:: bash

    $ pip list
    Package           Version
    ----------------- -------
    calfem-python     3.5.10
    cycler            0.10.0
    gmsh              4.8.4
    kiwisolver        1.3.1
    matplotlib        3.4.3
    numpy             1.21.2
    Pillow            8.3.1
    pip               20.2.3
    PyOpenGL          3.1.5
    pyparsing         2.4.7
    PyQt5             5.15.4
    PyQt5-Qt5         5.15.2
    PyQt5-sip         12.9.0
    PyQtWebEngine     5.15.4
    PyQtWebEngine-Qt5 5.15.2
    python-dateutil   2.8.2
    PyVTK             0.5.18
    scipy             1.7.1
    setuptools        49.2.1
    six               1.16.0
    visvis            1.13.0

We now have an exact copy of the myenv environment. This can be useful to recreate the requiremenets for a scientific software package on a different system or resource.