 #***********************************************************************************
 #   CMakeLists.txt  --  This file is part of cudatest.                             #
 #                                                                                  #
 #   MIT License                                                                    #
 #                                                                                  #
 #   Copyright (c) 2018 Ivo Filot <ivo@ivofilot.nl>                                 #
 #                                                                                  #
 #   Permission is hereby granted, free of charge, to any person obtaining a copy   #
 #   of this software and associated documentation files (the "Software"), to deal  #
 #   in the Software without restriction, including without limitation the rights   #
 #   to use, copy, modify, merge, publish, distribute, sublicense, and/or sell      #
 #   copies of the Software, and to permit persons to whom the Software is          #
 #   furnished to do so, subject to the following conditions:                       #
 #                                                                                  #
 #   The above copyright notice and this permission notice shall be included in all #
 #   copies or substantial portions of the Software.                                #
 #                                                                                  #
 #   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR     #
 #   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,       #
 #   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE    #
 #   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER         #
 #   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,  #
 #   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE  #
 #   SOFTWARE.                                                                      #
 #                                                                                  #
 #***********************************************************************************/

# set minimum cmake requirements
cmake_minimum_required(VERSION 2.8)
project (cudatest)

# add custom directory to look for .cmake files
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake/modules )

# Enable release build
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to 'Release' as none was specified.")
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# Include libraries
find_package(PkgConfig REQUIRED)
find_package(CUDA 9.0 REQUIRED)
set(CUDA_SEPARABLE_COMPILATION ON)
set(CUDA_PROPAGATE_HOST_FLAGS OFF)
set(CUDA_HOST_COMPILER g++)
pkg_check_modules(EIGEN eigen3 REQUIRED)

# Set include folders
include_directories(${CMAKE_CURRENT_SOURCE_DIR}
                    ${CMAKE_BINARY_DIR}
                    ${EIGEN_INCLUDE_DIRS})

# Add sources
file(GLOB SOURCES "*.cpp")
file(GLOB CUDA_FILES "src/" *.cu)

CUDA_ADD_LIBRARY(cudatest_gpu ${CUDA_FILES})

# Set C++14
add_definitions(-std=c++14)

# Link libraries
SET(CMAKE_EXE_LINKER_FLAGS "-Wl,-rpath=\$ORIGIN/lib")
add_executable(cudatest ${SOURCES})
target_link_libraries(cudatest cudatest_gpu)
