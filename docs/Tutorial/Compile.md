# Compile

## Prerequisites

1. To configure the source code, [CMake](https://cmake.org/download/) shall be available. Please download and install it before configuring the source code package.
2. The linear algebra driver used is [OpenBLAS](https://github.com/xianyi/OpenBLAS). You may want to compile it with the optimal configuration based on the specific machine.
3. It is strongly recommended to install Intel MKL for potentially better performance.
4. Please be aware that MKL is throttled on AMD platforms. Performance comparisons can be seen for example [here](https://github.com/flame/blis/blob/master/docs/Performance.md). If you have AMD CPUs please collect more knowledge to determine which linear algebra library is more suitable.
5. The `MUMPS` library may fail to compile on the first run due to dependency between modules. Build one more time to resolve the dependency problem.

## Toolset

A number of new features from new standards are utilized. To compile the binary, a compiler that supports **C++17** is required.

GCC, MSVC and Intel compilers are tested with the source code. Clang is not tested and thus not recommended.

On Windows, Visual Studio 2019 with Intel Parallel Studio is recommended. Alternatively, [WinLibs](http://winlibs.com/) can be used if GCC compilers are preferred.

On other platforms (Linux and MacOS), simply use GCC (at least version 9.3.0) which comes with a valid Fortran compiler.

## Obtain Source Code

Download the source code archive from GitHub [Releases](https://github.com/TLCFEM/suanPan/releases) or the latest [code](https://github.com/TLCFEM/suanPan/archive/master.zip).

## Configure and Compile

### Visual Studio

A solution file is provided under `MSVC/suanPan` folder. There are two configurations:

1. `Debug`: Assume no available Fortran compiler, all Fortran related libraries are provided as precompiled DLLs. Use OpenBLAS for linear algebra. Multithreading disabled. Visualisation disabled. HDF5 support disabled.
2. `Release`: Fortran libraries are configured with Intel compilers. Use MKL for linear algebra. Multithreading enabled. Visualisation enabled with VTK version 9.0. HDF5 support enabled. CUDA enabled.

If Intel oneAPI Toolkit and CUDA are not installed, only the `Debug` configuration can be successfully compiled. Simply open the solution, ignore all potential warnings and build the solution.

To compile `Release` version, please

1. Make sure oneAPI both base and HPC toolsets are installed. The MKL is enabled via integrated option `<UseInteloneMKL>Parallel</UseInteloneMKL>`.

2. Make sure CUDA is installed. The environment variable `$(CUDA_PATH)` is used to locate headers.

3. Make sure VTK is available. Define two environment variables `VTK_INC` and `VTK_LIB`, which point to include and library folders. On my machine, they are
   
   ```powershell
   VTK_INC=C:\Program Files\VTK\include\vtk-9.0
   VTK_LIB=C:\Program Files\VTK\lib
   ```
   
   For versions other than 9.0, names of the linked libraries shall be manually changed as they contain version numbers.

Alternatively, `CMake` can be used to generate solution files if some external packages are not available.

### Linux

The following instructions are based on Ubuntu. [CMake](https://cmake.org/) is used to manage builds. It is recommended to use **CMake** GUI if appropriate.

1. Install necessary tools.
   
   ```bash
   sudo apt install gcc g++ gfortran git cmake
   ```

2. Clone the project.
   
   ```bash
   git clone -b master https://github.com/TLCFEM/suanPan.git
   ```

3. Create build folder and configure via CMake. The default configuration disables parallelism `-DBUILD_MULTITHREAD=OFF` and enables HDF5 via bundled library `-DUSE_HDF5=ON`. Please check `.config.cmake` file or use GUI for available options.
   
   ```bash
   cd suanPan
   mkdir build
   cd build
   cmake ../
   ```

4. Invoke `make`.
   
   ```bash
   make -j4
   ```

Check the following record.

[![asciicast](https://asciinema.org/a/382311.svg)](https://asciinema.org/a/382311)

#### Install VTK

The following guide is based on Ubuntu.

1. Install OpenGL first, as well as compilers if necessary.
   
   ```bash
   sudo apt install gcc g++ gfortran libglu1-mesa-dev freeglut3-dev mesa-common-dev
   ```

2. Obtain VTK source code and unpack.
   
   ```bash
   wget https://www.vtk.org/files/release/9.0/VTK-9.0.0.tar.gz
   tar -xf VTK-9.0.0.tar.gz
   ```

3. Create folder for building VTK.
   
   ```bash
   mkdir VTK-build && cd VTK-build
   ```

4. Configure and compile VTK library. If necessary, installation destination can be modified. It is recommended to build shared libraries.
   
   ```bash
   cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON ../VTK-9.0.0
   sudo make install -j4
   ```

5. Now obtain `suanPan` source code and unpack it. To configure it with VTK support, users may use the following flag `-DUSE_EXTERNAL_VTK=ON`. If `FindVTK` is presented and `VTK` is installed to default location, there is no need to provide the variable `VTK_DIR`, otherwise point it to the `lib/cmake/vtk-9.0` folder.

#### Install MKL

The provided CMake configuration covers both `oneMKL 2021` and `Intel MKL 2020`. Please note MKL is included in oneAPI toolkit starting from 2021, which has a different folder structure compared to Intel Parallel Studio.

The following guide is a manual installation is based on Ubuntu command line.

1. Download MKL package. The link may differ from the given one for different versions. Please find the correct one first.
   
   ```bash
   wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/16533/l_mkl_2020.1.217.tgz
   ```

2. Unpack.
   
   ```bash
   tar -xzf l_mkl_2020.1.217.tgz && cd l_mkl_2020.1.217
   ```

3. Modify installation configurations and install.
   
   ```bash
   sed -i 's/decline/accept/g' ./silent.cfg
   ./install.sh --silent ./silent.cfg
   ```

4. Now compile `suanPan` by enabling MKL via option `-DUSE_MKL=ON`. The corresponding `MKLROOT` shall be assigned, for example `-DMKLROOT=/opt/intel/compilers_and_libraries/linux/mkl`, depending on the installation location.

### Example Configuration

The following command is used to compile the program to be distributed via snap.

```bash
# assume current folder is suanPan/build
# the parent folder contains source code
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_MULTITHREAD=ON -DUSE_HDF5=ON -DUSE_EXTERNAL_VTK=ON -DUSE_MKL=ON -DMKLROOT=/opt/intel/compilers_and_libraries/linux/mkl ..
```
