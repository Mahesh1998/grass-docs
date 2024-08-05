Building the grass using CMake in Ubuntu using system libraries
===============================================================

OS Information
--------------
Ubuntu Release 24.04

Verify if cmake exists
----------------------

.. code-block:: bash

   cmake --version

Output

.. code-block::

   Command 'cmake' not found, but can be installed with:
   sudo snap install cmake  # version 3.30.1, or
   sudo apt  install cmake  # version 3.27.8-1build1
   See 'snap info cmake' for additional versions.


Install CMake
-------------

.. code-block:: bash

   sudo apt update
   sudo apt upgrade
   sudo apt install cmake

create build directory and build using cmake

.. code-block:: bash

   mkdir build_dir
   cd build_dir
   cmake ..
   cmake --build .

Now it will prompt for unavailable dependencies and install all the dependencies.

Install dependencies for grass.
-------------------------------

.. code-block:: bash

   sudo apt-get install g++ \
                        flex \
                        bison \
                        libproj-dev \
                        libgdal-dev \
                        libx11-dev \
                        libgl1-mesa-dev \
                        libcairo2-dev \
                        gettext \
                        libfftw3-dev

Error1:
-------
Unable to install pdal using apt

.. code-block::

    sudo apt-get install libpdal-dev
   [sudo] password for mahesh: 
   Reading package lists... Done
   Building dependency tree... Done
   Reading state information... Done
   E: Unable to locate package libpdal-dev

Solution1:
----------
Install PDAL dependency manually

.. code-block:: bash

   git clone https://github.com/PDAL/PDAL.git
   cd PDAL
   mkdir build
   cd build
   cmake ..
   make
   sudo make install

Error2:
-------

.. code-block::

   [ 18%] Building C object display/CMakeFiles/d.grid.dir/d.grid/fiducial.c.o
   In file included from /home/mahesh/Documents/grass/display/d.grid/local_proto.h:1,
                 from /home/mahesh/Documents/grass/display/d.grid/fiducial.c:7:
   /home/mahesh/Documents/grass/build/output/lib/grass85/include/grass/gprojects.h:57:10: fatal error: ogr_srs_api.h: No such file or directory
   57 | #include <ogr_srs_api.h>
      |          ^~~~~~~~~~~~~~~
   compilation terminated.
   make[2]: *** [display/CMakeFiles/d.grid.dir/build.make:76: display/CMakeFiles/d.grid.dir/d.grid/fiducial.c.o] Error 1
   make[1]: *** [CMakeFiles/Makefile2:6336: display/CMakeFiles/d.grid.dir/all] Error 2
   make: *** [Makefile:146: all] Error 2

(or)

.. code-block::

   [ 47%] Building C object raster/CMakeFiles/r.path.dir/r.path/main.c.o
   In file included from /home/mahesh/Documents/grass/build/output/lib/grass85/include/grass/vect/digit.h:3,
                 from /home/mahesh/Documents/grass/build/output/lib/grass85/include/grass/vector.h:4,
                 from /home/mahesh/Documents/grass/raster/r.path/main.c:35:
   /home/mahesh/Documents/grass/build/output/lib/grass85/include/grass/vect/dig_structs.h:27:10: fatal error: ogr_api.h: No such file or directory
   27 | #include <ogr_api.h>
      |          ^~~~~~~~~~~
   compilation terminated.
   make[2]: *** [raster/CMakeFiles/r.path.dir/build.make:76: raster/CMakeFiles/r.path.dir/r.path/main.c.o] Error 1
   make[1]: *** [CMakeFiles/Makefile2:10958: raster/CMakeFiles/r.path.dir/all] Error 2
   make: *** [Makefile:146: all] Error 2

Solution2:
---------
Include GDAL in display/CMakelists.txt(d.grid), raster/CMakelists.txt(r.path) to fix the error. Similar errors occurred in multiple files and we included GDAL in required CMakelists based on the error received.

Error3:
-------

.. code-block::

   /home/mahesh/Documents/grass/lib/ogsf/gs2.c:40:10: fatal error: GL/glu.h: No such file or directory
      40 | #include <GL/glu.h>
         |          ^~~~~~~~~~
   compilation terminated.
   make[2]: *** [lib/CMakeFiles/grass_ogsf.dir/build.make:174: lib/CMakeFiles/grass_ogsf.dir/ogsf/gs2.c.o] Error 1
   make[1]: *** [CMakeFiles/Makefile2:4704: lib/CMakeFiles/grass_ogsf.dir/all] Error 2
   make: *** [Makefile:146: all] Error 2

Solution3:
---------
Here OpenGL is a system library is installed without GLU. So, we added condition to run OpenGL only if it founds GLU, GLX.

.. code-block::

   if(WITH_OPENGL AND OPENGL_GLU_FOUND AND OpenGL_GLX_FOUND)


Error4:
-------

.. code-block::

   /usr/bin/ld: CMakeFiles/r.info.dir/r.info/main.c.o: undefined reference to symbol 'json_object_set_string@@JSONC_0.14'
   /usr/bin/ld: /lib/x86_64-linux-gnu/libjson-c.so.5: error adding symbols: DSO missing from command line
   collect2: error: ld returned 1 exit status
   make[2]: *** [raster/CMakeFiles/r.info.dir/build.make:116: output/lib/grass85/bin/r.info] Error 1
   make[1]: *** [CMakeFiles/Makefile2:10330: raster/CMakeFiles/r.info.dir/all] Error 2
   make: *** [Makefile:146: all] Error 2

Solution4:
---------
This issue is fixed in the slackware sys library setup. So, pulled latest changes to the local branch.

Error5:
-------

.. code-block::

   CMake Error at cmake/modules/build_module.cmake:160 (message):
    grass_ogsf not a target
   Call Stack (most recent call first):
    cmake/modules/build_program.cmake:10 (build_module)
    cmake/modules/build_program_in_subdir.cmake:17 (build_program)
    misc/CMakeLists.txt:9 (build_program_in_subdir)

Solution5:
----------
Replaced WITH_OPENGL to grass_ogsf

Old Code that caused the error.

.. code-block::

   if(WITH_OPENGL)
   endif(WITH_OPENGL)

Updated Code to fix the issue.

.. code-block::

   if(TARGET grass_ogsf)
   endif(TARGET grass_ogsf)

Error6:
-------

.. code-block::

   make[2]: *** No rule to make target 'm.nviz.image', needed by 'CMakeFiles/ALL_MODULES'.  Stop.
   make[1]: *** [CMakeFiles/Makefile2:2602: CMakeFiles/ALL_MODULES.dir/all] Error 2
   make: *** [Makefile:146: all] Error 2

Solution6:
----------
Remove all the build files, including cache and rerun.

.. code-block:: bash
   
   rm -rf *

Error7:
-------

.. code-block::

   Traceback (most recent call last):
     File "/home/mahesh/Documents/grass/gui/wxpython/core/menutree.py", line 41, in <module>
       import wx
   ModuleNotFoundError: No module named 'wx'
   make[2]: *** [gui/wxpython/CMakeFiles/build_menustrings.dir/build.make:70: gui/wxpython/CMakeFiles/build_menustrings] Error 1
   make[1]: *** [CMakeFiles/Makefile2:22366: gui/wxpython/CMakeFiles/build_menustrings.dir/all] Error 2
   make: *** [Makefile:146: all] Error 2

Solution7:
----------
Install wxpython system library

.. code-block:: bash

   sudo apt install python3-wxgtk4.0


