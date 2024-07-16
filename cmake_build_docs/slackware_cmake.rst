Building the grass using CMake in Slackware using System Libraries
==================================================================

1. Create a build directory if doesn't exists in project directory and navigate to that build directory.

  | `mkdir build`
  | `cd build`

2. Run cmake to configure the project and generate a native build system. This will create a make file.
  `cmake ..`

3. Encountered below errors while running `make` and used the mentioned solutions to fix those errors.

Error1:
------
undefined reference to symbol 'json_object_set_string@@JSONC_0.14'
/usr/bin/ld: /usr/lib64/libjson-c.so.5: error adding symbols: DSO missing from command line

Solution1:
---------
Referenced grass_parson in CMakeLists in raster(r.info, r.profile) and vector(v.info) files

Error2:
------
Traceback (most recent call last):
  File "/home/mahesh98/usr/local/src/grass/gui/wxpython/core/menutree.py", line 41, in <module>
    import wx
ModuleNotFoundError: No module named 'wx'
make[2]: *** [gui/wxpython/CMakeFiles/build_menustrings.dir/build.make:70: gui/wxpython/CMakeFiles/build_menustrings] Error 1
make[1]: *** [CMakeFiles/Makefile2:22560: gui/wxpython/CMakeFiles/build_menustrings.dir/all] Error 2
make: *** [Makefile:146: all] Error 2


Solution2:
---------
| Created conda environment with the name grass and installed wxpython package.

  | `conda create -n grass`
  | `conda install wxpython=4.2.1`

