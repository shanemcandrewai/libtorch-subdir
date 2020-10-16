# PyTorch libtorch build example 

This CMakeLists.txt manages the building of a minimal libtorch from [PyTorch 1.6](https://github.com/pytorch/pytorch/tree/1.6) by modifying paths in the PyTorch source tree and loading the main [CMakeLists.txt](https://github.com/pytorch/pytorch/blob/v1.6.0/CMakeLists.txt) using `add_subdirectory(${PYTORCH_SRC_DIR} ${CMAKE_BINARY_DIR}/pytorch)`

## Prerequisites

1. Clone [PyTorch 1.6](https://github.com/pytorch/pytorch/tree/1.6) and adjust the [CMakeLists.txt](CMakeLists.txt) variable `PYTORCH_SRC_DIR` to point to the local repository, for example `set(PYTORCH_SRC_DIR ../pytorch)`

2. Install the [PyTorch prerequisites](https://github.com/pytorch/pytorch/tree/1.6#from-source)

## Usage
### Generate the project buildsystem
    cmake -S . -B build
#### Cmake options
##### RESET
This restores the PyTorch source working tree from HEAD and reapplies the modifications to paths. This can be enabled by passing the option `-D RESET=1`.
###### NO_BUILD_SHARED_LIBS (dependent on RESET)
The build generates a shared library by default. This can be disabled by passing the option `-D RESET=1 -D NO_BUILD_SHARED_LIBS=1`.
##### CMAKE_BUILD_TYPE 
The default buid type is `Release`. For a debug build pass option `-D CMAKE_BUILD_TYPE=Debug`
##### CMAKE_CXX_FLAGS
###### GCC
`-Og` enables optimizations that do not interfere with debugging
### Build the project
    cmake --build build
### Location of built libraries
    build/lib
### Cleaning / trouble-shooting
#### Linux
    rm build/CMakeCache.txt
    rm -rf build
#### Windows
    del build/CMakeCache.txt
    rmdir /s /q build
    mklink /d build d:\build
#### Restore PyTorch source without using standard ignore rules
    git clean -dfx
