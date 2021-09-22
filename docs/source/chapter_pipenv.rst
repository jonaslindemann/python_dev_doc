Using pipenv to manage environments and packages
================================================

**virtualenv**, **venv** and pip are essential tools for creating a reproducable environment for Python applications. However, they are separate tools which require many manual steps to setup a working environment for an application. 

**Pipenv** is a tool that combines package and environment creation in a single tool and automatically handles requirements.

Advantages with *Pipenv* are:

 * Single coool to manage packages and virtual environments
 * Better handling of requirements 
 * Everything is hashed to ensure security. 
 * Make sure the latest packages are used.

Installing pipenv
-----------------

**Pipenv** can be installed using the following commands:

.. code-block:: bash

    $ pip install pipenv

It can also be installed in your home directory using:

.. code-block:: bash

    $ pip install --user pipenv

Setting up a pipenv project
---------------------------

**Pipenv** is best used in a project setting. That is all project related source code and files are located in a single directory. This directory will be the base for the **Pipenv** environment and package data. An example of how to setup a project with **Pipenv** is shown below:

.. code-block:: bash

    $ mkdir myproj
    $ cd myproj
    $ pipenv install

    Creating a virtualenv for this project...
    Pipfile: E:\Users\Jonas\Development\python_dev_doc\examples\myproj\Pipfile
    Using E:/Program Files (x86)/Microsoft Visual Studio/Shared/Python37_64/python.exe (3.7.8) to create virtualenv...
    [   =] Creating virtual environment...created virtual environment CPython3.7.8.final.0-64 in 4414ms
    creator CPython3Windows(dest=C:\Users\Jonas Lindemann\.virtualenvs\myproj-jNDwD6z3, clear=False, no_vcs_ignore=False, global=False)        
    seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=C:\Users\Jonas Lindemann\AppData\Local\pypa\virtualenv)
        added seed packages: pip==21.1.3, setuptools==57.4.0, wheel==0.36.2
    activators BashActivator,BatchActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator

    Successfully created virtual environment!
    Virtualenv location: C:\Users\Jonas Lindemann\.virtualenvs\myproj-jNDwD6z3
    Creating a Pipfile for this project...
    Pipfile.lock not found, creating...
    Locking [dev-packages] dependencies...
    Locking [packages] dependencies...
    Updated Pipfile.lock (a65489)!
    Installing dependencies from Pipfile.lock (a65489)...
    ================================ 0/0 - 00:00:00
    To activate this project\'s virtualenv, run pipenv shell.
    Alternatively, run a command inside the virtualenv with pipenv run.    

Please note that on Windows it used the default Python installation on your system and not the Python interpreter in the **PATH**. If you want to build the environment using a specific Python interpreter use the following command instead:

.. code-block:: bash

    $ pipenv --python [path to python interpreter] install

This will create 2 files in the project directory. A *Pipfile* and a *Pipfile.lock*. The *Pipfile* contains the dependencies for the project and the *Pipfile.lock* contains the latest tested combination of packages.

A typical *Pipfile* is shown below:

.. code-block:: bash

    [[source]]
    url = "https://pypi.org/simple"
    verify_ssl = true
    name = "pypi"

    [packages]
    calfem-python = "*"

    [dev-packages]

    [requires]
    python_version = "3.8"


Installing packages
-------------------

Packages can be installed in the project using the **pipenv install** command. In the following command we install the calfem-python package in the current pipenv project.

.. code-block:: bash

    $ cd myproj
    $ pipenv install calfem-python

    Installing calfem-python...
    Adding calfem-python to Pipfile\'s [packages]...
    Installation Succeeded
    Pipfile.lock (db4242) out of date, updating to (656452)...
    Locking [dev-packages] dependencies...
    Locking [packages] dependencies...
            Building requirements...
    Resolving dependencies...
    Success!
    Updated Pipfile.lock (656452)!
    Installing dependencies from Pipfile.lock (656452)...
    ================================ 0/0 - 00:00:00
    To activate this project\'s virtualenv, run pipenv shell.
    Alternatively, run a command inside the virtualenv with pipenv run.
    This will add calfem-python to the 

Running a project using pipenv
------------------------------

We now have a project that can use the calfem-python moduel with all its dependencies. We now create a main python file, *fea_analysis.py* in our project directory with the following contents:

.. code-block:: python

    import numpy as np
    import calfem.core as cfc

    # ----- Topology -------------------------------------------------

    Edof = np.array([
        [1, 2, 3, 4, 5, 6],
        [4, 5, 6, 7, 8, 9],
        [7, 8, 9, 10, 11, 12]
    ])

    # ----- Stiffness matrix K and load vector f ---------------------

    K = np.mat(np.zeros((12,12)))
    f = np.mat(np.zeros((12,1)))
    f[4] = -10000.

    # ----- Element stiffness matrices  ------------------------------

    E = 2.1e11
    A = 45.3e-4
    I = 2510e-8
    ep = np.array([E,A,I])
    ex = np.array([0.,3.])
    ey = np.array([0.,0.])

    Ke = cfc.beam2e(ex,ey,ep)

    print(Ke)

    # ----- Assemble Ke into K ---------------------------------------

    K = cfc.assem(Edof,K,Ke);

    # ----- Solve the system of equations and compute support forces -

    bc = np.array([1,2,11])
    (a,r) = cfc.solveq(K,f,bc);

    # ----- Section forces -------------------------------------------

    Ed=cfc.extractEldisp(Edof,a);

    es1, ed1, ec1 = cfc.beam2s(ex, ey, ep, Ed[0,:], nep=10)
    es2, ed2, ec2 = cfc.beam2s(ex, ey, ep, Ed[1,:], nep=10)
    es3, ed3, ec3 = cfc.beam2s(ex, ey, ep, Ed[2,:], nep=10)

    # ----- Results --------------------------------------------------

    print("a=")
    print(a)
    print("r=")
    print(r)
    print("es1=")
    print(es1)
    print("es2=")
    print(es2)
    print("es3=")
    print(es3)

    print("ed1=")
    print(ed1)
    print("ed2=")
    print(ed2)
    print("ed3=")
    print(ed3)

It is possible to run the project by issuing the following command:

.. code-block:: bash

    $ pipenv run fea_analysis.py
    [[ 3.17100000e+08  0.00000000e+00  0.00000000e+00 -3.17100000e+08
     0.00000000e+00  0.00000000e+00]
     [ 0.00000000e+00  2.34266667e+06  3.51400000e+06  0.00000000e+00
     -2.34266667e+06  3.51400000e+06]
     
     ...
     
     [ 0.         -0.01992032]
     [ 0.         -0.01823785]]
    ed3=
    [[ 0.00000000e+00 -1.99203187e-02]
     [ 0.00000000e+00 -1.82378462e-02]
     [ 0.00000000e+00 -1.63679985e-02]
     [ 0.00000000e+00 -1.43341976e-02]
     [ 0.00000000e+00 -1.21598653e-02]
     [ 0.00000000e+00 -9.86842362e-03]
     [ 0.00000000e+00 -7.48329434e-03]
     [ 0.00000000e+00 -5.02789938e-03]
     [ 0.00000000e+00 -2.52566063e-03]
     [ 0.00000000e+00  1.73472348e-18]
     [ 0.00000000e+00  2.52566063e-03]]    

Executing a shell in the created environment
--------------------------------------------

It is also possible to create a shell with the created project environment. From this shell it is possible to examine the installed packaged using the **pipenv graph**:

.. code-block:: bash

    $ pipenv shell
    Launching subshell in virtual environment...
    (myproj-jNDwD6z3) (base) $ pip list
    Package           Version
    ----------------- -------
    calfem-python     3.5.10
    cycler            0.10.0
    gmsh              4.8.4
    kiwisolver        1.3.1
    matplotlib        3.4.3
    numpy             1.21.2
    Pillow            8.3.1
    pip               21.1.1
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
    setuptools        56.0.0
    six               1.16.0
    visvis            1.13.0
    wheel             0.36.2

When you don't want to use the environment anymore just type **exit** and you are back in your previous environment.

Listing package dependencies
----------------------------

Using the **pipenv graph** it is also possible to list the dependencies of a project:

.. code-block:: bash

    $ pipenv graph
    calfem-python==3.5.10
        - gmsh [required: Any, installed: 4.8.4]
        - matplotlib [required: Any, installed: 3.4.3]
            - cycler [required: >=0.10, installed: 0.10.0]
            - six [required: Any, installed: 1.16.0]
            - kiwisolver [required: >=1.0.1, installed: 1.3.1]
            - numpy [required: >=1.16, installed: 1.21.2]
            - pillow [required: >=6.2.0, installed: 8.3.1]
            - pyparsing [required: >=2.2.1, installed: 2.4.7]
            - python-dateutil [required: >=2.7, installed: 2.8.2]
            - six [required: >=1.5, installed: 1.16.0]
        - numpy [required: Any, installed: 1.21.2]
        - pyqt5 [required: Any, installed: 5.15.4]
            - PyQt5-Qt5 [required: >=5.15, installed: 5.15.2]
            - PyQt5-sip [required: >=12.8,<13, installed: 12.9.0]
        - pyqtwebengine [required: Any, installed: 5.15.4]
            - PyQt5 [required: >=5.15.4, installed: 5.15.4]
            - PyQt5-Qt5 [required: >=5.15, installed: 5.15.2]
            - PyQt5-sip [required: >=12.8,<13, installed: 12.9.0]
            - PyQt5-sip [required: >=12.8,<13, installed: 12.9.0]
            - PyQtWebEngine-Qt5 [required: >=5.15, installed: 5.15.2]
        - pyvtk [required: Any, installed: 0.5.18]
            - six [required: Any, installed: 1.16.0]
        - scipy [required: Any, installed: 1.7.1]
            - numpy [required: >=1.16.5,<1.23.0, installed: 1.21.2]
        - visvis [required: Any, installed: 1.13.0]
            - numpy [required: Any, installed: 1.21.2]
            - pyOpenGl [required: Any, installed: 3.1.5]

This command does not require the project environment to be activated.

Updating packages in a project
------------------------------

Installed packages in a project can be updated using the **pipenv update** command in your project directory.

.. code-block:: bash

    $ pipenv update --outdated
    Locking...Building requirements...
    Resolving dependencies...
    Success!
    Skipped Update of Package visvis: 1.13.0 installed,, 1.13.0 available.
    Skipped Update of Package six: 1.16.0 installed,, 1.16.0 available.
    Skipped Update of Package scipy: 1.7.1 installed,, 1.7.1 available.
    Skipped Update of Package PyVTK: 0.5.18 installed,, 0.5.18 available.
    Skipped Update of Package python-dateutil: 2.8.2 installed,, 2.8.2 available.
    Skipped Update of Package PyQtWebEngine: 5.15.4 installed,, 5.15.4 available.
    Skipped Update of Package PyQtWebEngine-Qt5: 5.15.2 installed,, 5.15.2 available.
    Skipped Update of Package PyQt5: 5.15.4 installed,, 5.15.4 available.
    Skipped Update of Package PyQt5-sip: 12.9.0 installed,, 12.9.0 available.
    Skipped Update of Package PyQt5-Qt5: 5.15.2 installed,, 5.15.2 available.
    Skipped Update of Package pyparsing: 2.4.7 installed,, 2.4.7 available.
    Skipped Update of Package PyOpenGL: 3.1.5 installed,, 3.1.5 available.
    Skipped Update of Package Pillow: 8.3.1 installed,, 8.3.1 available.
    Skipped Update of Package numpy: 1.21.2 installed,, 1.21.2 available.
    Skipped Update of Package matplotlib: 3.4.3 installed,, 3.4.3 available.
    Skipped Update of Package kiwisolver: 1.3.1 installed,, 1.3.1 available.
    Skipped Update of Package gmsh: 4.8.4 installed,, 4.8.4 available.
    Skipped Update of Package cycler: 0.10.0 installed,, 0.10.0 available.
    Skipped Update of Package calfem-python: 3.5.10 installed, 3.5.10 required (Unpinned in Pipfile), 3.5.10 available.
    All packages are up to date!    