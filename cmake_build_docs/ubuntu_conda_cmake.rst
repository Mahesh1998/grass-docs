Building the grass using CMake in Ubuntu using Conda packages
=============================================================


Installing Dependencies
-----------------------

Install Miniconda:
Navigate to https://docs.anaconda.com/miniconda/miniconda-install/ and select the operating system as Linux to get detailed steps of installing Miniconda. 


Add Conda forge channel to default channels

.. code-block:: bash

   conda config --append channels conda-forge

cmake, make, gxx, flex, bison, proj4, gdal, xorg-libx11, pyopengl ,cairo, gettext, fftw, pdal

Installing Dependencies:
---------------------------------------


.. code-block:: bash

   conda install cmake
   conda install make
   conda install gxx
   conda install flex
   conda install bison
   conda install proj
   conda install libgdal
   conda install xorg-libx11
   conda install gettext
   conda install fftw
   conda install blas
   conda install libpdal

PDAL downgrades sqlite. So, we are reinstalling manually as upgrade makes sqlite to broke.

.. code-block:: bash

   conda install libsqlite --force-reinstall

Error1:
-------
Unable to find openGL

Solution1:
----------
Running without opengl

.. code-block:: bash

   cmake -DWITH_OPENGL=NO ..


Error2:
-------

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

Solution2:
----------
Added environment variables

.. code-block:: bash

   export LD_LIBRARY_PATH=/home/user1/miniconda3/envs/env_name/lib


Error3:
-------

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

Solution3:
----------
Referenced grass_parson in CMakeLists in raster(r.univar, r3.univar) and vector(v.univar)

Error4:
-------

[100%] Built target gui_images
Traceback (most recent call last):
  File "/home/mahesh/Documents/grass/gui/wxpython/core/menutree.py", line 41, in <module>
    import wx
ModuleNotFoundError: No module named 'wx'
make[2]: *** [gui/wxpython/CMakeFiles/build_menustrings.dir/build.make:70: gui/wxpython/CMakeFiles/build_menustrings] Error 1
make[1]: *** [CMakeFiles/Makefile2:22387: gui/wxpython/CMakeFiles/build_menustrings.dir/all] Error 2
make: *** [Makefile:134: all] Error 2

Solution4:
----------

.. code-block:: bash

   conda install wxpython

