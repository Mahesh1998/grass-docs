Building the grass using CMake in Ubuntu
========================

Installing Dependencies
-----------------------

Install Miniconda:
Navigate to https://docs.anaconda.com/miniconda/miniconda-install/ and select the operating system as Linux to get detailed steps of installing Miniconda. 


Add Conda forge channel to default channels

 conda config --append channels conda-forge



Install CMake in Ubuntu
-----------------------
1. sudo apt update
2. sudo apt upgrade
3. sudo apt install cMake

Install dependencies for grass. g++, flex, bison,PROJ_LIBRARY, GDAL, X11, OpenGL, Cairo, Gettext, FFTW
Similar conda packages are gcc, flex, bison, proj4, gdal, xorg-libx11, pyopengl ,cairo, gettext, fftw, pdal
    sudo apt-get install g++ &&
    sudo apt-get install flex &&
    sudo apt-get install bison &&
    sudo apt-get install libproj-dev &&
    sudo apt-get install libgdal-dev &&
    sudo apt-get install libx11-dev &&
    sudo apt-get install libgl1-mesa-dev &&
    sudo apt-get install libcairo2-dev &&
    sudo apt-get install gettext &&
    sudo apt-get install libfftw3-dev &&

Install PDAL dependency manually

git clone https://github.com/PDAL/PDAL.git
    cd PDAL
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

create build directory and build using cmake
mkdir build_dir
cd build_dir
cmake ..
cmake --build .

