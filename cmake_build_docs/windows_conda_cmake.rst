Building the grass using CMake in Windows using Conda packages
=============================================================

Install visual Studio with c++ compiler
---------------------------------------

Follow this documentation to install https://learn.microsoft.com/en-us/cpp/build/vscpp-step-0-installation?view=msvc-170


Installing Miniconda
-----------------------

Install Miniconda:
Navigate to https://docs.anaconda.com/miniconda/miniconda-install/ and select the operating system as Windows to get detailed steps of installing Miniconda. 

Now open Developer command prompt for visual studio and activate conda using below command.

.. code-block:: bash
   conda activate

Add Conda forge channel to default channels and set strict priority

.. code-block:: bash

   conda config --add channels conda-forge
   conda config --set channel_priority strict


Installing Dependencies
-----------------------
   
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



Error 3:
---------
.. code-block::

   Creating C:/Users/mahesh98/Documents/grass/build/output/lib/grass85/docs/html/d.background.html
   | was unexpected at this time.
   C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Microsoft\VC\v170\Microsoft.CppCommon.targets(254,5): error MSB8066: Custom build for 'C:\Users\mahesh98\Documents\grass\build\CMakeFiles\a83b921f437a0cdfb379a66f175f9077\d.background_to_translate.c.rule;C:      \Users\mahesh98\Documents\grass\build\CMakeFiles\6899dabf2590f66e7f425183cdcf84b8\d.background.html.rule;C:\Users\mahesh98\Documents\grass\build\CMakeFiles\7db07849a4df18736333a9e6a1325bcb\d.background.rule;C:\Users\mahesh98\Documents\grass\scripts\CMakeLists.txt' exited with code 255.
   Done building project "d.background.vcxproj" -- FAILED.


Solution3:
---------

grass/cmake/modules/build_script_in_subdir.cmake

Old Code:

.. code-block::

   add_custom_command(
        OUTPUT ${OUT_HTML_FILE}
        COMMAND ${CMAKE_COMMAND} -E copy ${G_SRC_DIR}/${G_NAME}.html
                ${CMAKE_CURRENT_BINARY_DIR}/${G_NAME}.html
        COMMAND
          ${grass_env_command} ${PYTHON_EXECUTABLE}
          ${OUTDIR}/${G_DEST_DIR}/${G_NAME}${SCRIPT_EXT}
          --html-description < /dev/null | grep -v
          '</body>\|</html>\|</div> <!-- end container -->' > ${TMP_HTML_FILE}
        COMMAND ${grass_env_command} ${PYTHON_EXECUTABLE} ${MKHTML_PY} ${G_NAME} >
                ${OUT_HTML_FILE}
        COMMAND ${copy_images_command}
        COMMAND ${CMAKE_COMMAND} -E remove ${TMP_HTML_FILE}
                ${CMAKE_CURRENT_BINARY_DIR}/${G_NAME}.html
        COMMENT "Creating ${OUT_HTML_FILE}"
        DEPENDS ${TRANSLATE_C_FILE} LIB_PYTHON)


New Code:

.. code-block::

   if(WIN32)
      add_custom_command(
        OUTPUT ${OUT_HTML_FILE}
        COMMAND ${CMAKE_COMMAND} -E copy ${G_SRC_DIR}/${G_NAME}.html
                ${CMAKE_CURRENT_BINARY_DIR}/${G_NAME}.html
        COMMAND
          ${grass_env_command} ${PYTHON_EXECUTABLE}
          ${OUTDIR}/${G_DEST_DIR}/${G_NAME}${SCRIPT_EXT}
          --html-description < nul | findstr /V
          "</body>\|</html>\|</div> <!-- end container -->" > ${TMP_HTML_FILE}
        COMMAND ${grass_env_command} ${PYTHON_EXECUTABLE} ${MKHTML_PY} ${G_NAME} >
                ${OUT_HTML_FILE}
        COMMAND ${copy_images_command}
        COMMAND ${CMAKE_COMMAND} -E remove ${TMP_HTML_FILE}
                ${CMAKE_CURRENT_BINARY_DIR}/${G_NAME}.html
        COMMENT "Creating ${OUT_HTML_FILE}"
        DEPENDS ${TRANSLATE_C_FILE} LIB_PYTHON)
    else()
      add_custom_command(
        OUTPUT ${OUT_HTML_FILE}
        COMMAND ${CMAKE_COMMAND} -E copy ${G_SRC_DIR}/${G_NAME}.html
                ${CMAKE_CURRENT_BINARY_DIR}/${G_NAME}.html
        COMMAND
          ${grass_env_command} ${PYTHON_EXECUTABLE}
          ${OUTDIR}/${G_DEST_DIR}/${G_NAME}${SCRIPT_EXT}
          --html-description < /dev/null | grep -v
          '</body>\|</html>\|</div> <!-- end container -->' > ${TMP_HTML_FILE}
        COMMAND ${grass_env_command} ${PYTHON_EXECUTABLE} ${MKHTML_PY} ${G_NAME} >
                ${OUT_HTML_FILE}
        COMMAND ${copy_images_command}
        COMMAND ${CMAKE_COMMAND} -E remove ${TMP_HTML_FILE}
                ${CMAKE_CURRENT_BINARY_DIR}/${G_NAME}.html
        COMMENT "Creating ${OUT_HTML_FILE}"
        DEPENDS ${TRANSLATE_C_FILE} LIB_PYTHON)
    endif()

Error 4:
---------

Need to update

Solution:
---------

Old Code:

.. code-block::

   #ifdef POINT2D_C
   struct line_pnts *Pnts;
   struct line_cats *Cats2;
   dbDriver *driver2;
   dbString sql2;
   struct Map_info Map2;
   struct field_info *ff;
   int count;

   #else
   extern struct line_pnts *Pnts;
   extern struct line_cats *Cats2;
   extern dbDriver *driver2;
   extern dbString sql2;
   extern struct Map_info Map2;
   extern struct field_info *ff;
   extern int count;
   #endif

New Code:

.. code-block::

   #ifdef POINT2D_C
   GRASS_INTERPFL_EXPORT struct line_pnts *Pnts;
   GRASS_INTERPFL_EXPORT struct line_cats *Cats2;
   GRASS_INTERPFL_EXPORT dbDriver *driver2;
   GRASS_INTERPFL_EXPORT dbString sql2;
   GRASS_INTERPFL_EXPORT struct Map_info Map2;
   GRASS_INTERPFL_EXPORT struct field_info *ff;
   GRASS_INTERPFL_EXPORT int count;
   #else
   GRASS_INTERPFL_EXPORT extern  struct line_pnts *Pnts;
   GRASS_INTERPFL_EXPORT extern  struct line_cats *Cats2;
   GRASS_INTERPFL_EXPORT extern dbDriver *driver2;
   GRASS_INTERPFL_EXPORT extern dbString sql2;
   GRASS_INTERPFL_EXPORT extern struct Map_info Map2;
   GRASS_INTERPFL_EXPORT extern struct field_info *ff;
   GRASS_INTERPFL_EXPORT extern int count;
   #endif


Error 5: 
---------

.. code-block::

   Python Files opening in default editor instead of execution for module build_modules_items_xml due to empty value in GRASS_PYTHON.

Solution:
---------

grass/cmake/modules/build_gui_in_subdr.cmake
grass/CMakeLists.txt

Old Code:

.. code-block::

   set(PGM_NAME ${G_NAME})
   configure_file(${CMAKE_SOURCE_DIR}/cmake/windows_launch.bat.in
	${OUTDIR}/${GRASS_INSTALL_SCRIPTDIR}/${G_NAME}.bat @ONLY)


   set(grass_env_command
       ${CMAKE_COMMAND} -E env "PATH=${BIN_DIR}${sep}${SCRIPTS_DIR}${sep}${env_path}${sep}${LIB_DIR}"
       "PYTHONPATH=${ETC_PYTHON_DIR}${sep}${GUI_WXPYTHON_DIR}${sep}$ENV{PYTHONPATH}"
       "GISBASE=${RUN_GISBASE_NATIVE}" "GISRC=${GISRC}" "LC_ALL=C" "LANG=C"
       "LANGUAGE=C" "MODULE_TOPDIR=${MODULE_TOPDIR}" "HTMLDIR=${DOC_DIR}"
       "LC_ALL=C" "LANG=C"
       "LANGUAGE=C"
       "VERSION_NUMBER=\"${GRASS_VERSION_NUMBER}\""
       "VERSION_DATE=\"${GRASS_VERSION_DATE}\"")


New Code:
---------

.. code-block::

   set(PGM_NAME ${G_TARGET_NAME})
   configure_file(${CMAKE_SOURCE_DIR}/cmake/windows_launch.bat.in
	${OUTDIR}/${GRASS_INSTALL_SCRIPTDIR}/${G_TARGET_NAME}.bat @ONLY)

   set(grass_env_command
       ${CMAKE_COMMAND} -E env "PATH=${BIN_DIR}${sep}${SCRIPTS_DIR}${sep}${env_path}${sep}${LIB_DIR}"
       "PYTHONPATH=${ETC_PYTHON_DIR}${sep}${GUI_WXPYTHON_DIR}${sep}$ENV{PYTHONPATH}"
       "GRASS_PYTHON=${PYTHON_EXECUTABLE}" "GISBASE=${RUN_GISBASE_NATIVE}" "GISRC=${GISRC}" "LC_ALL=C" "LANG=C"
       "LANGUAGE=C" "MODULE_TOPDIR=${MODULE_TOPDIR}" "HTMLDIR=${DOC_DIR}"
       "LC_ALL=C" "LANG=C"
       "LANGUAGE=C"
       "VERSION_NUMBER=\"${GRASS_VERSION_NUMBER}\""
       "VERSION_DATE=\"${GRASS_VERSION_DATE}\"")



Error 6: WXPython
------------------

.. code-block::

   ------ Build started: Project: build_menustrings, Configuration: Debug x64 ------
   Traceback (most recent call last):
     File "C:\Users\mahesh98\Documents\grass\gui\wxpython\core\menutree.py", line 41, in <module>
       import wx
   ModuleNotFoundError: No module named 'wx'
   C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Microsoft\VC\v170\Microsoft.CppCommon.targets(254,5): error MSB8066: Custom build for 'C:\Users\mahesh98\Documents\grass\build\CMakeFiles\4e1e9302f1f4c9a0a99455bea3c23162\build_menustrings.rule;C:\Users\mahesh98\Documents\grass\gui\wxpython\CMakeLists.txt' exited with code 1.
   Done building project "build_menustrings.vcxproj" -- FAILED.


Solution:
---------

.. code-block::

   conda install wxpython



Error while installing wxpython

.. code-block::

   Could not solve for environment specs
   The following packages are incompatible
   ├─ pin-1 is installable and it requires
   │  └─ python 3.13.* , which can be installed;
   └─ wxpython 4.2.1**  is not installable because there are no viable options
      ├─ wxpython 4.2.1 would require
      │  └─ python >=3.10,<3.11.0a0 , which conflicts with any installable versions previously reported;
      ├─ wxpython 4.2.1 would require
      │  └─ python >=3.11,<3.12.0a0 , which conflicts with any installable versions previously reported;
      ├─ wxpython 4.2.1 would require
      │  └─ python >=3.12,<3.13.0a0 , which conflicts with any installable versions previously reported;
      ├─ wxpython 4.2.1 would require
      │  └─ python >=3.8,<3.9.0a0 , which conflicts with any installable versions previously reported;
      └─ wxpython 4.2.1 would require
         └─ python >=3.9,<3.10.0a0 , which conflicts with any installable versions previously reported.




Recommended python version is 3.12

.. code-block::

   conda install python=3.12


Error 7: wxpython
------------------

.. code-block::

   ------ Build started: Project: build_menustrings, Configuration: Debug x64 ------
   Traceback (most recent call last):
     File "C:\Users\mahesh98\Documents\grass\gui\wxpython\core\menutree.py", line 44, in <module>
       from core.settings import UserSettings
     File "C:\Users\mahesh98\Documents\grass\build\output\lib\grass85\gui\wxpython\core\settings.py", line 30, in <module>
       from core.gcmd import GException, GError
     File "C:\Users\mahesh98\Documents\grass\build\output\lib\grass85\gui\wxpython\core\gcmd.py", line 41, in <module>
       from win32file import ReadFile, WriteFile
   ModuleNotFoundError: No module named 'win32file'
   C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Microsoft\VC\v170\Microsoft.CppCommon.targets(254,5): error MSB8066: Custom build for 'C:\Users\mahesh98\Documents\grass\build\CMakeFiles\4e1e9302f1f4c9a0a99455bea3c23162\build_menustrings.rule' exited with code 1.

Solution:
---------

.. code-block::

   conda install pywin32



Error 8: R.IN.PDAL
------------------

.. code-block::

   ------ Build started: Project: v.in.pdal, Configuration: Debug x64 ------
   Building Custom Rule C:/Users/mahesh98/Documents/grass/vector/CMakeLists.txt
   filters.c
   lidar.c
   projection.c
   Generating Code...
   main.cpp
   C:\Users\mahesh98\Documents\grass\vector\v.in.pdal\main.cpp(267,32): error C2065: 'F_OK': undeclared identifier
   C:\Users\mahesh98\Documents\grass\vector\v.in.pdal\main.cpp(267,9): error C3861: 'access': identifier not found
   Done building project "v.in.pdal.vcxproj" -- FAILED.

Solution:
---------

grass/vector/v.in.pdal/main.cpp

Old Code:

.. code-block::

   extern "C" {
   #include <grass/gis.h>
   #include <grass/vector.h>
   #include <grass/gprojects.h>
   #include <grass/glocale.h>
   }

New Code:

.. code-block::

   extern "C" {
   #include <grass/gis.h>
   #include <grass/vector.h>
   #include <grass/gprojects.h>
   #include <grass/glocale.h>
   #include <unistd.h>
   }

Error 9: r3.mapcalc
------------------

.. code-block::

   ------ Build started: Project: r3.mapcalc, Configuration: Debug x64 ------
   [BISON][mapcalc.tab.c] Building parser with bison 3.8.2
   [FLEX][mapcalc.yy.c] Building scanner with win_flex 2.6.4
   Building Custom Rule C:/Users/mahesh98/Documents/grass/raster/r.mapcalc/CMakeLists.txt
   cl : command line  warning D9025: overriding '/openmp' with '/openmp:llvm'
   column_shift.c
   evaluate.c
   expression.c
   function.c
   main.c
   xrowcol.c
   mapcalc.tab.c
   mapcalc.yy.c
   map3.c
   xarea.c
   xcoor3.c
   xres3.c
   Generating Code...
   xarea.obj : error LNK2001: unresolved external symbol columns
   xcoor3.obj : error LNK2001: unresolved external symbol columns
   xres3.obj : error LNK2001: unresolved external symbol columns
   column_shift.obj : error LNK2001: unresolved external symbol columns
   evaluate.obj : error LNK2001: unresolved external symbol columns
   xrowcol.obj : error LNK2001: unresolved external symbol columns
   map3.obj : error LNK2001: unresolved external symbol columns
   C:\Users\mahesh98\Documents\grass\build\output\lib\grass85\bin\r3.mapcalc.exe : fatal error LNK1120: 1 unresolved externals
   Done building project "r3.mapcalc.vcxproj" -- FAILED.

Solution:
---------

grass/raster/r.mapcalc/map3.c

Old Code:

.. code-block::

   static void prepare_region_from_maps(expression **, int, int);
   RASTER3D_Region current_region3;

New Code:

.. code-block::

   static void prepare_region_from_maps(expression **, int, int);
   int columns;
   RASTER3D_Region current_region3;


Error 10: v.profile
------------------

.. code-block::

   ------ Build started: Project: v.profile, Configuration: Debug x64 ------
   Building Custom Rule C:/Users/mahesh98/Documents/grass/vector/CMakeLists.txt
   main.c
   processors.c
   Generating Code...
   main.obj : error LNK2019: unresolved external symbol db_close_cursor referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_close_database_shutdown_driver referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_convert_column_value_to_string referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_describe_table referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_fetch referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_column_name referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_column_sqltype referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_cursor_table referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_num_rows referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_string referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_table_column referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_get_table_number_of_columns referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_init_handle referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_init_string referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_open_database referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_open_select_cursor referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_select_int referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_set_handle referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_set_string referenced in function main
   main.obj : error LNK2019: unresolved external symbol db_start_driver referenced in function main
   C:\Users\mahesh98\Documents\grass\build\output\lib\grass85\bin\v.profile.exe : fatal error LNK1120: 20 unresolved externals
   Done building project "v.profile.vcxproj" -- FAILED.


Solution:
---------

grass/vector/CMakeLists.txt

Old Code:

.. code-block::

   build_program_in_subdir(v.profile DEPENDS grass_gis grass_vector GDAL)

New Code:

.. code-block::

   build_program_in_subdir(
     v.profile 
     DEPENDS
     grass_dbmibase
     grass_dbmiclient
     grass_gis
     grass_vector
     GDAL)


Error 11: Build PSO
------------------

.. code-block::

   ------ Build started: Project: build_pso, Configuration: Debug x64 ------
   man generation: parser standard options
   usage: parser_standard_options.py [-h] [-f {html,csv,grass}] [-l URL]
                                  [-t TEXT] [-o OUTPUT] [-s STARTSWITH]
                                  [-p HTMLPARMAS]
   parser_standard_options.py : error : unrecognized arguments: class=scroolTable'
   C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Microsoft\VC\v170\Microsoft.CppCommon.targets(254,5): error MSB8066: Custom build for 'C:\Users\mahesh98\Documents\grass\build\CMakeFiles\a18c54ca0aa7a475bab83671248a50ad\build_pso.rule;C:\Users\mahesh98\Documents\grass\man\CMakeLists.txt' exited with code 2.
   Done building project "build_pso.vcxproj" -- FAILED.

Solution:
---------

grass/man/CMakeLists.txt

Old Code:

.. code-block::

   'id="opts_table" class="scroolTable"'

New Code:

.. code-block::

   "id='opts_table' class='scroolTable'"

Error 12: r_colors_thumbnails
-----------------------------

.. code-block::

   ------ Build started: Project: r_colors_thumbnails, Configuration: Debug x64 ------
   Creating thumbnails
   CUSTOMBUILD : error : An error occurred while running r.mapcalc with expression:
          tmp_grad_rel_9684 = float(col())
   CUSTOMBUILD : error : Unable to compile pattern <tmp\.thumbnails\.py\.9684>
   CUSTOMBUILD : error : Unable to compile pattern <tmp\_grad\_rel\_9684>
   Exception ignored in atexit callback: <function cleanup at 0x000001C324CDA200>
   Traceback (most recent call last):
     File "C:\Users\mahesh98\Documents\grass\build\utils\thumbnails.py", line 27, in cleanup
       gs.run_command(
     File "C:\Users\mahesh98\Documents\grass\build\output\lib\grass85\etc\python\grass\script\core.py", line 485, in run_command
       return handle_errors(returncode, result=None, args=args, kwargs=kwargs)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     File "C:\Users\mahesh98\Documents\grass\build\output\lib\grass85\etc\python\grass\script\core.py", line 364, in handle_errors
       raise CalledModuleError(module=module, code=code, returncode=returncode)
   grass.exceptions.CalledModuleError: Module run `g.remove --q -f type=raster name=tmp_grad_rel_9684` ended with an error.
   The subprocess ended with a non-zero return code: 1. See errors above the traceback or in the error output.   
   C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Microsoft\VC\v170\Microsoft.CppCommon.targets(254,5): error MSB8066: Custom build for 'C:\Users\mahesh98\Documents\grass\build\CMakeFiles\9e808aaa1404748559fd68b637b99dd6\r_colors_thumbnails.rule;C:\Users\mahesh98\Documents\grass\CMakeLists.txt' exited with code 1.
   Done building project "r_colors_thumbnails.vcxproj" -- FAILED.



Solution:
---------

Issue Still Exists



