cmake_minimum_required(VERSION 3.18)

project(maxflow-cuda LANGUAGES CXX CUDA)

# Check GCC
set(GCC_REQUIRED_VERSION 9)
if (NOT ((CMAKE_CXX_COMPILER_ID STREQUAL "GNU") AND (CMAKE_CXX_COMPILER_VERSION MATCHES "^${GCC_REQUIRED_VERSION}*")))
    message(FATAL_ERROR "Required GCC ${GCC_REQUIRED_VERSION}")
endif ()

# Check NVCC
set(CUDA_REQUIRED_VERSION 11)
if (NOT ((CMAKE_CUDA_COMPILER_ID STREQUAL "NVIDIA") AND (CMAKE_CUDA_COMPILER_VERSION MATCHES "^${CUDA_REQUIRED_VERSION}*")))
    message(FATAL_ERROR "Required CUDA ${CUDA_REQUIRED_VERSION}")
endif ()

# CUDA
set(CMAKE_CUDA_STANDARD 17)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)
set(CMAKE_CUDA_ARCHITECTURES 86)
string(APPEND CMAKE_CUDA_FLAGS " --expt-extended-lambda")
set(CMAKE_CUDA_SEPARABLE_COMPILATION ON)
string(APPEND CMAKE_CUDA_FLAGS " -Xcompiler -Wall")
string(APPEND CMAKE_CUDA_FLAGS_DEBUG " -G")
string(APPEND CMAKE_CUDA_FLAGS_RELEASE " -lineinfo")

message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

# Add sources
set(SOURCES
    src/global_relabel.cu
    src/graph_s.cpp
    src/io_par.cu
    src/main.cu
    src/preflow.cu
    src/push_relabel.cu
    src/push_relabel_kernel.cu include/utils.cuh)

# Add include path
include_directories(include)

# Add target executable
add_executable(maxflow-cuda ${SOURCES})
