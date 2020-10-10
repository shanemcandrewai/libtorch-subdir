# PyTorch libtorch build example 

This CMakeLists.txt manages the building of a minimal libtorch from [PyTorch 1.6](https://github.com/pytorch/pytorch/tree/1.6) by modifying paths in the PyTorch source tree and loading the main [CMakeLists.txt](https://github.com/pytorch/pytorch/blob/v1.6.0/CMakeLists.txt) using `add_subdirectory(${PYTORCH_SRC_DIR} ${CMAKE_BINARY_DIR}/pytorch`

## Prerequisites

1. Clone [PyTorch 1.6](https://github.com/pytorch/pytorch/tree/1.6) and adjust the [CMakeLists.txt](CMakeLists.txt) variable `PYTORCH_SRC_DIR` to point to the local repository, for example `set(PYTORCH_SRC_DIR ../pytorch)`

2. Install the [PyTorch prerequisites](https://github.com/pytorch/pytorch/tree/1.6#from-source)

## Usage
### build the library
#### Linux
    rm -rf build && cmake -S . -B build
#### Windows
    rmdir /s /q build && cmake -S . -B build
#### Cmake options
##### RESET
This restores the PyTorch source working tree from HEAD and reapplies the modifications to paths. This can be enabled by passing the option `-D RESET=1`.
##### NO_BUILD_SHARED_LIBS (dependent on RESET)
The build does not generate a shared library by default. This can be disabled by passing the option `-D RESET=1 -D NO_BUILD_SHARED_LIBS=0`.
