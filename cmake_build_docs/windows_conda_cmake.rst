Building the grass using CMake in Windows using Conda packages
=============================================================

Install visual Studio with c++ compiler
---------------------------------------

Follow this documentation to install https://learn.microsoft.com/en-us/cpp/build/vscpp-step-0-installation?view=msvc-170


Installing Dependencies
-----------------------

Install Miniconda:
Navigate to https://docs.anaconda.com/miniconda/miniconda-install/ and select the operating system as Windows to get detailed steps of installing Miniconda. 


Add Conda forge channel to default channels

.. code-block:: bash

   conda config --append channels conda-forge


Installing Dependencies:
---------------------------------------
   
Installing dependencies based on the error messages received while compiling grass.


.. code-block:: bash

   conda install cmake
   conda install winflexbison
   conda install proj
   conda install libgdal
   conda install pcre
   conda install fftw
   conda install blas-devel
   conda install pdal
   conda install git


Error1:
-------

.. code-block::

   -- Creating C:/Users/mahesh98/Documents/grass/build/output/lib/grass85/include/grass/config.h
   CMake Error at cmake/modules/build_module.cmake:160 (message):
     LIBM not a target
   Call Stack (most recent call first):
     lib/vector/diglib/CMakeLists.txt:28 (build_module)

Solution1:
---------

Old Code

.. code-block::

   if(UNIX)
     find_library(MATH_LIBRARY m)
     add_library(LIBM INTERFACE IMPORTED GLOBAL)
     set_property(TARGET LIBM PROPERTY INTERFACE_LINK_LIBRARIES ${MATH_LIBRARY})
     mark_as_advanced(M_LIBRARY)
   endif()

Updated Code

.. code-block::

   find_library(MATH_LIBRARY m)
   add_library(LIBM INTERFACE IMPORTED GLOBAL)
   set_property(TARGET LIBM PROPERTY INTERFACE_LINK_LIBRARIES ${MATH_LIBRARY})
   mark_as_advanced(M_LIBRARY)

Removed condition, so that the LIBM target is added for all operating systems.

Error2:
-------

.. code-block::

   -- Configuring done (7.4s)
   CMake Error at cmake/modules/build_module.cmake:105 (add_executable):
     Cannot find source file:
  
       timer/msvc/gettimeofday.c
  
      Tried extensions .c .C .c++ .cc .cpp .cxx .cu .mpp .m .M .mm .ixx .cppm .h
     .hh .h++ .hm .hpp .hxx .in .txx .f .F .for .f77 .f90 .f95 .f03 .hip .ispc
   Call Stack (most recent call first):
     cmake/modules/build_program.cmake:10 (build_module)
     cmake/modules/build_program_in_subdir.cmake:14 (build_program)
     utils/CMakeLists.txt:12 (build_program_in_subdir)
  
  
   CMake Error at cmake/modules/build_module.cmake:105 (add_executable):
     No SOURCES given to target: current_time_s_ms
   Call Stack (most recent call first):
     cmake/modules/build_program.cmake:10 (build_module)
     cmake/modules/build_program_in_subdir.cmake:14 (build_program)
     utils/CMakeLists.txt:12 (build_program_in_subdir)


Solution2:
---------

Added gettimeofday.c from https://github.com/postgres/postgres/blob/master/src/port/win32gettimeofday.c by using old commit message as reference.


