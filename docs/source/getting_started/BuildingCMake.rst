Building MFiX-Exa with cmake
============================

CMake build is a two-step process. First ``cmake`` is invoked to create
configuration files and makefiles in a chosen directory (``builddir``).
Next, the actual build is performed by invoking ``make`` from within ``builddir``.
If you are new to CMake, `this short tutorial <https://hsf-training.github.io/hsf-training-cmake-webpage/>`_
from the HEP Software foundation is the perfect place to get started with it.


The CMake build process for MFiX-Exa is summarized as follows:

.. highlight:: console

::

    >> mkdir /path/to/builddir
    >> cd    /path/to/builddir
    >> cmake [options] -DCMAKE_BUILD_TYPE=[Debug|Release|RelWithDebInfo|MinSizeRel] /path/to/mfix
    >> make

In the above snippet, ``[options]`` indicates one or more options for the
customization of the build, as described later in this section.
If the option ``CMAKE_BUILD_TYPE`` is omitted,
``CMAKE_BUILD_TYPE=Release`` is assumed.

There are two modes to build MFiX-Exa with cmake:

o **SUPERBUILD (recommended):** The AMReX and AMReX-Hydro git repos are cloned and built as part
of the MFiX-Exa build process and placed in the ``mfix/subprojects`` directory.
Each of these repos can be manipulated like a regular git repo
(you can change branches, hashes, add remotes, etc.)
This method is strongly encouraged as it ensures that the configuration options
for MFiX-Exa are compatible with the AMReX and AMReX-Hydro hashes that are checked out.

o **STANDALONE:** MFiX-Exa source code is built separately and linked to existing
AMReX and AMReX-Hydro repos. This is ideal for continuous integration severs (CI)
and regression testing applications. AMReX and AMReX_Hydro library versions and
configuration options must meet MFiX-Exa requirements.

.. note::
   **MFiX-Exa requires CMake 3.20 or higher.**

.. _sec:build:superbuild:

SUPERBUILD Instructions (recommended)
-------------------------------------

By default MFiX-Exa CMake looks for an existing AMReX installation on the system. If none is found, it falls back to **SUPERBUILD** mode.
In this mode, MFiX-Exa CMake inherents AMReX CMake targets and configuration options, that is, MFiX-Exa and AMReX are configured and built as a single entity

Assuming no valid AMReX installation is present on the target system, and ``AMReX_ROOT`` is not set (see :ref:`sec:build:standalone`), the following code will build MFiX-Exa in **SUPERBUILD** mode:

.. code:: shell

    > git clone http://mfix.netl.doe.gov/gitlab/exa/mfix.git
    > cd mfix
    > mkdir build
    > cd build
    > cmake [mfix options] [amrex options] -DCMAKE_BUILD_TYPE=[Debug|Release|RelWithDebInfo|MinSizeRel] ..
    > make -j

``[amrex options]`` in the snippet above is a list of any of the AMReX configuration options listed in
the `AMReX user guide <https://amrex-codes.github.io/amrex/docs_html/BuildingAMReX.html#building-with-cmake>`_,
while ``[mfix options]`` is any of the configuration options listed :ref:`here <tab:mfixcmakeoptions>`.


For example, to enable AMReX profiling capabilities in MFiX_Exa, configure as follows:

.. code:: shell

    > cmake [mfix options] -DAMReX_TINY_PROFILE=yes -DCMAKE_BUILD_TYPE=[Debug|Release|RelWithDebInfo|MinSizeRel] ..



Working with the AMReX submodule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**SUPERBUILD** mode relies on a git submodule to checkout the AMReX git repository.
If the AMReX submodule is not initialized, **SUPERBUILD** mode will initialize it and checkout
the AMReX commit the submodule is pointing to.
Instead, if the AMReX submodule has already been manually initialized and a custom commit has been checked out,
**SUPERBUILD** mode will use that commit. For MFiX-Exa development or testing, you may need to build with a different
branch or release of AMReX.

The ``subprojects/amrex`` directory is a Git repo, so use all standard Git
commands. Either ``cd subprojects/amrex`` to run Git commands in the ``amrex``
directory, or use ``git -C subprojects/amrex`` in the MFiX-Exa repo. For
instance, to build with the ``my-amrex-branch`` branch of the AMReX repo:

.. code:: shell

    > cd /path/to/mfix
    > git -C subprojects/amrex checkout my-amrex-branch
    > git status
    ...
    modified:   subprojects/amrex (new commits)

The branch ``my-amrex-branch`` will then be used when building MFiX-Exa.

To revert to the default version of the AMReX submodule, run ``git submodule
update``:

.. code:: shell

    > cd /path/to/mfix
    > git submodule update

You can edit, commit, pull, and push AMReX changes from ``subprojects/amrex``.
AMReX development is outside the scope of this document. Run ``git status`` in
the top-level MFix-Exa repo to see whether the AMReX submodule has new commits,
modified files, or untracked files.

To update the AMReX submodule referenced by MFiX-Exa:

.. code:: shell

    > git -C subprojects/amrex checkout UPDATED_AMREX_COMMIT_SHA1
    > git add subprojects/amrex
    > git commit -m 'Updating AMReX version'

This will only update the AMReX SHA-1 referenced by MFiX-Exa. Uncommitted AMReX
changes and untracked AMReX files under ``subprojects/amrex`` are not added by
``git add subprojects/amrex``. (To commit to the AMReX repo, change directories
to ``subprojects/amrex`` and run Git commands there, before ``git add
subprojects/amrex``.)

.. note::

    Only update the AMReX submodule reference in coordination with the other
    MFiX-Exa developers!


.. _sec:build:standalone:

**STANDALONE** instructions
---------------------------------------------------------------------

Building AMReX
~~~~~~~~~~~~~~~~~~~

Clone AMReX from the official Git repository.
Note that the only branch available is *development*:

.. code:: shell

    > git clone https://github.com/AMReX-Codes/amrex.git

Next, configure, build and install AMReX as follows:

.. code:: shell

    > cd amrex
    > mkdir build
    > cd build
    > cmake -DCMAKE_BUILD_TYPE=[Debug|Release|RelWithDebInfo|MinSizeRel] -DAMReX_PARTICLES=yes -DAMReX_EB=yes -DAMReX_PLOTFILE_TOOLS=yes [other amrex options] -DCMAKE_INSTALL_PREFIX:PATH=/absolute_path_to_amrex_installdir ..
    > make install

The options **AMReX\_PARTICLES=yes**, **AMReX\_EB=yes** and  **AMReX\_PLOTFILE\_TOOLS=yes** are required by MFiX-Exa. ``[other amrex options]`` in the snippet above refers to any other AMReX configuration option in addition to the required ones. Please refer to the `AMReX user guide <https://amrex-codes.github.io/amrex/docs_html/BuildingAMReX.html#building-with-cmake>`_ for more details on building AMReX with CMake.


Building MFiX-Exa
~~~~~~~~~~~~~~~~~

Clone and build MFiX-Exa:

.. code:: shell

    > git clone http://mfix.netl.doe.gov/gitlab/exa/mfix.git
    > mkdir build
    > cd build
    > cmake -DCMAKE_BUILD_TYPE=[Debug|Release|RelWithDebInfo|MinSizeRel] [mfix options] -DAMReX_ROOT=/absolute/path/to/amrex/installdir ..
    > make -j


Passing ``-DAMReX_ROOT=/absolute/path/to/amrex/installdir`` instructs CMake to search
``/absolute/path/to/amrex/installdir`` before searching system paths
for an available AMReX installation.
``AMReX_ROOT`` can also be set as an environmental variable instead of passing it as a command line option.
``[mfix options]`` indicates any of the configuration option listed in the table below.


.. _tab:mfixcmakeoptions:

.. table:: MFiX-Exa configuration options

           +-----------------+------------------------------+------------------+-------------+
           | Option name     | Description                  | Possible values  | Default     |
           |                 |                              |                  | value       |
           +=================+==============================+==================+=============+
           | CMAKE\_CXX\     | User-defined C++ flags       | valid C++        | None        |
           | _FLAGS          |                              | compiler flags   |             |
           +-----------------+------------------------------+------------------+-------------+
           | CMAKE\_CUDA\    | User-defined CUDA flags      | valid CUDA       | None        |
           | _FLAGS          |                              | compiler flags   |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_MPI       | Enable build with MPI        | no/yes           | yes         |
           |                 |                              |                  |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_OMP       | Enable build with OpenMP     | no/yes           | no          |
           |                 |                              |                  |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_GPU\_     | On-node, accelerated GPU \   | NONE,SYSCL,\     | NONE        |
           | BACKEND         | backend                      | CUDA,HIP         |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_HYPRE     | Enable HYPRE support         | no/yes           | no          |
           |                 |                              |                  |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_FPE       | Build with Floating-Point    | no/yes           | no          |
           |                 | Exceptions checks            |                  |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_CSG       | Build with CSG support       | no/yes           | no          |
           |                 |                              |                  |             |
           +-----------------+------------------------------+------------------+-------------+
           | MFIX\_MPI\_     | Concurrent MPI calls from    | no/yes           | no          |
           | THREAD\_MULTIPLE| multiple threads             |                  |             |
           |                 |                              |                  |             |
           |                 |                              |                  |             |
           +-----------------+------------------------------+------------------+-------------+



Few more notes on building MFiX-Exa
-----------------------------------

The system defaults compilers can be overwritten as follows:

.. code:: shell

    > cmake -DCMAKE_CXX_COMPILER=<c++-compiler> [options]  ..

When building on a platform that uses the ``module`` utility, use either
the above command (with full path to the compilers) or the following:

.. code:: shell

    > cmake -DCMAKE_CXX_COMPILER=CC [options] ..

MFiX-Exa uses the same compiler flags used to build AMReX, unless
``CMAKE_CXX_FLAGS`` is explicitly provided, or
the environmental variable ``CXXFLAGS`` is set.


For GPU builds, MFiX-Exa relies on the `AMReX GPU build infrastructure <https://amrex-codes.github.io/amrex/docs_html/GPU.html#building-with-cmake>`_
. The target architecture to build for can be specified via the AMReX configuration option ``-DAMReX_CUDA_ARCH=<target-architecture>``,
or by defining the *environmental variable* ``AMREX_CUDA_ARCH`` (all caps). If no GPU architecture is specified,
CMake will try to determine which GPU is supported by the system.


Building MFiX-Exa for Cori (NERSC)
-----------------------------------

Standard build
~~~~~~~~~~~~~~~~~~~

For the Cori cluster at NERSC, you first need to load/unload modules required to build MFIX-Exa.

.. code:: shell

    > module unload altd
    > module unload darshan
    > module load cmake/3.14.0

The default options for Cori are the **Haswell** architecture and **Intel** compiler, if you want to compile with the **Knight's Landing (KNL)** architecture:

.. code:: shell

    > module swap craype-haswell craype-mic-knl

Or use the **GNU** compiler:

.. code:: shell

    > module swap PrgEnv-intel PrgEnv-gnu

Now MFIX-Exa can be built following the :ref:`sec:build:superbuild`.

.. note::

    The load/unload modules options could be saved in the `~/.bash_profile.ext`


GPU build
~~~~~~~~~~~~~~~~~~~

To compile on the GPU nodes in Cori, you first need to purge your modules, most of which won't work on the GPU nodes

.. code:: shell

    > module purge

Then, you need to load the following modules:

.. code:: shell

    > module load modules esslurm gcc cuda openmpi/3.1.0-ucx cmake/3.14.0

Currently, you need to use OpenMPI; mvapich2 seems not to work.

Then, you need to use slurm to request access to a GPU node:

.. code:: shell

    > salloc -N 1 -t 02:00:00 -c 80 -C gpu -A m1759 --gres=gpu:8 --exclusive

This reservers an entire GPU node for your job. Note that you can’t cross-compile for the GPU nodes - you have to log on to one and then build your software.

Finally, navigate to the base of the MFIX-Exa repository and compile in GPU mode:

.. code:: shell

    > cd mfix
    > mdkir build
    > cd build
    > cmake -DMFIX_GPU_BACKEND=CUDA -DAMReX_CUDA_ARCH=Volta -DCMAKE_CXX_COMPILER=g++ ..
    > make -j

For more information about GPU nodes in Cori -- `<https://docs-dev.nersc.gov/cgpu/>`_

Building MFiX-Exa for Summit (OLCF)
-----------------------------------

For the Summit cluster at OLCF, you first need to load/unload modules required to build MFIX-Exa.

.. code:: shell

    > module unload xalt
    > module unload darshan
    > module load gcc
    > module load cmake/3.14.0

Now MFIX-Exa can be built following the :ref:`sec:build:superbuild`.

To build MFIX-Exa for GPUs, you need to load cuda module:

.. code:: shell

    > module load cuda/10.1.105

To compile:

.. code:: shell

    > cd mfix
    > mdkir build
    > cd build
    > cmake -DCMAKE_CXX_COMPILER=g++ -DMFIX_GPU_BACKEND=[NONE|CUDA]
    > make -j

An example of a *submission_script* for GPUs can be found in the repo ``mfix/tests/GPU_test/script.sh``.
For more information about Summit cluster: `<https://www.olcf.ornl.gov/for-users/system-user-guides/summit/>`_
