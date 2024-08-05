Building the grass using CMake in Ubuntu using Conda packages
=============================================================


Installing Dependencies
-----------------------

Install Miniconda:
Navigate to https://docs.anaconda.com/miniconda/miniconda-install/ and select the operating system as Linux to get detailed steps of installing Miniconda. 


Add Conda forge channel to default channels

.. code-block:: bash

   conda config --append channels conda-forge



Installing Dependencies:
---------------------------------------

.. code-block::
   
   grass/build$ cmake ..
   Command 'cmake' not found, but can be installed with:
   sudo snap install cmake  # version 3.30.1, or
   sudo apt  install cmake  # version 3.27.8-1build1
   See 'snap info cmake' for additional versions.

Installing dependencies based on the error messages received while compiling grass.


.. code-block:: bash

   conda install cmake
   conda install make
   conda install gxx
   conda install flex
   conda install bison
   conda install proj
   conda install libgdal
   conda install xorg-libx11
   conda install mesa-libgl-devel-cos7-x86_64
   conda install gettext
   conda install fftw
   conda install blas
   conda install libpdal

Error 1: Sqlite
---------------

.. code-block::

   [ 10%] Linking C executable ../../output/lib/grass85/driver/db/sqlite
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libz.so.1, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libgomp.so.1, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libzstd.so.1, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libiconv.so.2, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_isError'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_decompress'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `uncompress'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_compress'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `zError'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `libiconv'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_getErrorName'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `libiconv_open'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `compressBound@ZLIB_1.2.0'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `libiconv_close'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_compressBound'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `compress2'
   collect2: error: ld returned 1 exit status
   make[2]: *** [db/drivers/CMakeFiles/sqlite.dir/build.make:310: output/lib/grass85/driver/db/sqlite] Error 1
   make[1]: *** [CMakeFiles/Makefile2:5917: db/drivers/CMakeFiles/sqlite.dir/all] Error 2
   make: *** [Makefile:134: all] Error 2


Solution 1
----------

PDAL downgrades sqlite. So, we need to upgrade sqlite.

.. code-block::

   grass/build$ conda list sqlite
   # packages in environment at /home/mahesh/miniconda3/envs/grass:
   #
   # Name                    Version                   Build  Channel
   libsqlite                 3.46.0               hde9e2c9_0    conda-forge
   sqlite                    3.31.1               h7b6447c_0  

At this time, the latest version of sqlite is 3.46.0

.. code-block:: bash
   
   conda install sqlite=3.46.0


Error 2: Sqlite
---------------

.. code-block::

   [ 10%] Building C object db/drivers/CMakeFiles/sqlite.dir/sqlite/create_table.c.o
   In file included from /home/mahesh/Documents/grass/db/drivers/sqlite/create_table.c:17:
   /home/mahesh/Documents/grass/db/drivers/sqlite/globals.h:1:10: fatal error: sqlite3.h: No such file or directory
       1 | #include <sqlite3.h>
         |          ^~~~~~~~~~~
   compilation terminated.
   make[2]: *** [db/drivers/CMakeFiles/sqlite.dir/build.make:76: db/drivers/CMakeFiles/sqlite.dir/sqlite/create_table.c.o] Error 1
   make[1]: *** [CMakeFiles/Makefile2:5917: db/drivers/CMakeFiles/sqlite.dir/all] Error 2
   make: *** [Makefile:134: all] Error 2


Solution 2
----------

Upgrading of sqlite corrupts libsqlite. So, we are force-reinstalling.

.. code-block::

   conda install libsqlite --force-reinstall


Error 3: Sqlite
---------------

.. code-block::

   [ 10%] Linking C executable ../../output/lib/grass85/driver/db/sqlite
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libz.so.1, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libgomp.so.1, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libzstd.so.1, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: warning: libiconv.so.2, needed by ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev, not found (try using -rpath or -rpath-link)
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_isError'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_decompress'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `uncompress'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_compress'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `zError'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `libiconv'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_getErrorName'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `libiconv_open'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `compressBound@ZLIB_1.2.0'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `libiconv_close'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `ZSTD_compressBound'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: ../../output/lib/grass85/lib/libgrass_gis.so.8.5.0dev: undefined reference to `compress2'
   collect2: error: ld returned 1 exit status
   make[2]: *** [db/drivers/CMakeFiles/sqlite.dir/build.make:310: output/lib/grass85/driver/db/sqlite] Error 1
   make[1]: *** [CMakeFiles/Makefile2:5917: db/drivers/CMakeFiles/sqlite.dir/all] Error 2
   make: *** [Makefile:134: all] Error 2

Solution 3
----------

Added environment variables to pick conda libraries instead of system libraries.

.. code-block:: bash

   export LD_LIBRARY_PATH=/home/user1/miniconda3/envs/env_name/lib


Error 4: Parson
---------------

Wherever we get below error, I've added grass_parson to the respective CMakelists. 

.. code-block::

   [ 59%] Linking C executable ../../output/lib/grass85/bin/r.univar
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: CMakeFiles/r.univar.dir/stats.c.o: undefined reference to symbol 'json_object_set_string@@JSONC_0.14'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: /home/mahesh/miniconda3/envs/new_grass/lib/libjson-c.so.5: error adding symbols: DSO missing from command line
   collect2: error: ld returned 1 exit status
   make[2]: *** [raster/r.univar/CMakeFiles/r.univar.dir/build.make:134: output/lib/grass85/bin/r.univar] Error 1
   make[1]: *** [CMakeFiles/Makefile2:12989: raster/r.univar/CMakeFiles/r.univar.dir/all] Error 2
   make: *** [Makefile:134: all] Error 2

(or)

.. code-block::

   [ 85%] Linking C executable ../output/lib/grass85/bin/v.univar
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: CMakeFiles/v.univar.dir/v.univar/main.c.o: in function `summary':
   main.c:(.text+0x1e12): undefined reference to `json_value_init_object'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x1e4f): undefined reference to `json_object'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x1e81): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x1ebe): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x1eee): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x1f1c): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x1f4c): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: CMakeFiles/v.univar.dir/v.univar/main.c.o:main.c:(.text+0x1f6e): more undefined references to `json_object_set_number' follow
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: CMakeFiles/v.univar.dir/v.univar/main.c.o: in function `summary':
   main.c:(.text+0x2b85): undefined reference to `json_value_init_array'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2b95): undefined reference to `json_array'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2b9e): undefined reference to `json_value_init_object'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2bae): undefined reference to `json_object'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2be0): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2bff): undefined reference to `json_object_set_number'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2c12): undefined reference to `json_array_append_value'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2c2c): undefined reference to `json_object_set_value'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2f24): undefined reference to `json_serialize_to_string_pretty'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2f6d): undefined reference to `json_free_serialized_string'
   /home/mahesh/miniconda3/envs/new_grass/bin/../lib/gcc/x86_64-conda-linux-gnu/14.1.0/../../../../x86_64-conda-linux-gnu/bin/ld: main.c:(.text+0x2f79): undefined reference to `json_value_free'
   collect2: error: ld returned 1 exit status
   make[2]: *** [vector/CMakeFiles/v.univar.dir/build.make:104: output/lib/grass85/bin/v.univar] Error 1
   make[1]: *** [CMakeFiles/Makefile2:20701: vector/CMakeFiles/v.univar.dir/all] Error 2

Solution 4
----------

Referenced grass_parson in CMakeLists in raster(r.univar, r3.univar) and vector(v.univar)

Error 5: WXPython
-----------------

.. code-block::

   [100%] Built target gui_images
   Traceback (most recent call last):
     File "/home/mahesh/Documents/grass/gui/wxpython/core/menutree.py", line 41, in <module>
       import wx
   ModuleNotFoundError: No module named 'wx'
   make[2]: *** [gui/wxpython/CMakeFiles/build_menustrings.dir/build.make:70: gui/wxpython/CMakeFiles/build_menustrings] Error 1
   make[1]: *** [CMakeFiles/Makefile2:22387: gui/wxpython/CMakeFiles/build_menustrings.dir/all] Error 2
   make: *** [Makefile:134: all] Error 2

Solution 5
----------

.. code-block:: bash

   conda install wxpython
