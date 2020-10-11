cmake_minimum_required(VERSION 3.10)

project(libtorch_subdir)

set(PYTORCH_SRC_DIR ../pytorch)

include(CMakeDependentOption)
option(DEBUG "Debugging information" ON)
option(RESET "Restore and clean PyTorch source tree")
CMAKE_DEPENDENT_OPTION(NO_BUILD_SHARED_LIBS "Build libcaffe2.so" ON RESET OFF)
option(USE_CUDA "disable compiling CUDA, to save compile time")
option(USE_DISTRIBUTED "disable distributed (c10d, gloo, mpi, etc.) build")
option(USE_MKLDNN "disable using MKL-DNN")
option(USE_FBGEMM "disable using FBGEMM (quantized 8-bit server operators)")
option(USE_NNPACK "disable compiling with NNPACK")
option(USE_QNNPACK "disable QNNPACK build (quantized 8-bit operators)")
option(USE_XNNPACK "disable compiling with XNNPACK")
option(BUILD_CUSTOM_PROTOBUF "Build and use Caffe2's own protobuf under third_party")
option(BUILD_PYTHON "Build Python binaries")
option(BUILD_CAFFE2_OPS "Build Caffe2 operators")

if(DEBUG)
  set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Debug build" FORCE)
endif()

if(RESET)
  find_package(Git)
  if(Git_FOUND)
    execute_process(COMMAND ${GIT_EXECUTABLE} restore :/ WORKING_DIRECTORY ${PYTORCH_SRC_DIR})
  endif()

    file(READ ${PYTORCH_SRC_DIR}/cmake/Dependencies.cmake FILE_CONTENTS)
    string(REPLACE "add_subdirectory($\{CMAKE_SOURCE_DIR}/third_party/fmt" "add_subdirectory(${PYTORCH_SRC_DIR}/third_party/fmt ${CMAKE_BINARY_DIR}/third_party/fmt" FILE_CONTENTS "${FILE_CONTENTS}")
    file(WRITE ${PYTORCH_SRC_DIR}/cmake/Dependencies.cmake "${FILE_CONTENTS}")

    file(READ ${PYTORCH_SRC_DIR}/cmake/Codegen.cmake FILE_CONTENTS)
    string(REPLACE "set(GEN_COMMAND" "find_package(Python)\n  set(GEN_COMMAND" FILE_CONTENTS "${FILE_CONTENTS}")
    string(REPLACE "PYTHON_EXECUTABLE" "Python_EXECUTABLE" FILE_CONTENTS "${FILE_CONTENTS}")
    string(REPLACE "$\{CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/${PYTORCH_SRC_DIR}" FILE_CONTENTS "${FILE_CONTENTS}")
    string(REPLACE "CMAKE_PROJECT_NAME" "PROJECT_NAME" FILE_CONTENTS "${FILE_CONTENTS}")
    file(WRITE ${PYTORCH_SRC_DIR}/cmake/Codegen.cmake "${FILE_CONTENTS}")

    file(READ ${PYTORCH_SRC_DIR}/caffe2/core/common.h FILE_CONTENTS)
    string(REPLACE "caffe2/core/macros.h" "${CMAKE_BINARY_DIR}/caffe2/core/macros.h" FILE_CONTENTS "${FILE_CONTENTS}")
    file(WRITE ${PYTORCH_SRC_DIR}/caffe2/core/common.h "${FILE_CONTENTS}")

endif()

add_subdirectory(${PYTORCH_SRC_DIR} ${CMAKE_BINARY_DIR}/pytorch)
