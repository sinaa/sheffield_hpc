.. _`Intel R (Sharc)`:

R (Intel Build)
===============

.. sidebar:: R

   :URL: http://www.r-project.org/
   :Documentation: http://www.r-project.org/

R is a statistical computing language. This version of R is built using the :ref:`Intel compilers <iceberg_intel_compilers>` and the Intel Math Kernel Library. This combination can result in significantly faster runtimes in certain circumstances.

Most R extensions are written and tested for the gcc suite of compilers so it is recommended that you perform testing before switching to this version of R.

Loading the modules
-------------------
After connecting to Sharc (see :ref:`ssh`),  start an interactive session with the :code:`qrshx` command.

There are two types of the Intel builds of R, `sequential` and `parallel`.

`sequential` makes use of one CPU core and can be used as a drop-in replacement for the standard version of R installed on Sharc. ::

    module load apps/R/3.3.2/intel-17.0-sequential

The `parallel` version makes use of multiple CPU cores for certain linear algebra routines since it is linked to the parallel version of the Intel MKL. Note that **only** linear algebra routines are automatically parallelised.  ::

    module load apps/R/3.3.2/intel-17.0-parallel

When using the parallel module, you must also ensure that you set the bash environment variable `OMP_NUM_THREADS` to the number of cores you require and also use the smp parallel environment.  E.g. Add the following to your submission script ::

    #$ -pe smp 8
    export OMP_NUM_THREADS=8

    module load apps/R/3.3.2/intel-17.0-parallel

Installing additional packages
------------------------------
To ensure that the Intel builds do not contaminate the standard gcc builds, the Intel R module files set the environment variable `R_LIBS_USER` to point to `~/R/intel_R/parallel-3.3.2/` or `~/R/intel_R/sequential-3.3.2/` respectively.

As a user, you should not need to worry about this detail and just install packages as you usually would from within R. e.g. ::

    install.packages("dplyr")

The Intel builds of R will use the Intel compiler suite, instead of gcc and gfortran, to compile any C++ or Fortran extensions.
The Intel build of R will ignore any packages installed in your home directory for the standard version of R and vice versa

Installation Notes
------------------
These notes are primarily for administrators of the system.

**version 3.3.2**

This was a scripted install. It was compiled from source with Intel Compiler 17.0.0 and with `--enable-R-shlib` enabled. It was run in batch mode.

* :download:`intel-17.0-parallel.sh </sharc/software/install_scripts/apps/R/3.3.2/intel-17.0-parallel.sh>` Downloads, compiles, tests and installs R 3.3.2 using Intel Compilers and the parallel MKL.
* :download:`intel-17.0-sequential.sh </sharc/software/install_scripts/apps/R/3.3.2/intel-17.0-sequential.sh>` Downloads, compiles, tests and installs R 3.3.2 using Intel Compilers and the sequential MKL.
* :download:`intel-17.0-parallel </sharc/software/modulefiles/apps/R/3.3.2/intel-17.0-parallel>` Parallel Module File
* :download:`intel-17.0-sequential </sharc/software/modulefiles/apps/R/3.3.2/intel-17.0-sequential>` Sequential Module File
