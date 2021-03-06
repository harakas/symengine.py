cmake_minimum_required(VERSION 2.8)
project(python_wrapper)

set(CMAKE_PREFIX_PATH ${SymEngine_DIR} ${CMAKE_PREFIX_PATH})
find_package(SymEngine 0.1.0 REQUIRED CONFIG
             PATH_SUFFIXES lib/cmake/symengine cmake/symengine CMake/)
message("SymEngine_DIR : " ${SymEngine_DIR})
set(CMAKE_BUILD_TYPE ${SYMENGINE_BUILD_TYPE})
set(CMAKE_CXX_FLAGS_RELEASE ${SYMENGINE_CXX_FLAGS_RELEASE})
set(CMAKE_CXX_FLAGS_DEBUG ${SYMENGINE_CXX_FLAGS_DEBUG})
include_directories(${SYMENGINE_INCLUDE_DIRS})

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")
find_package(Python REQUIRED)
find_package(Cython REQUIRED)

if(HAVE_SYMENGINE_MPC OR MPC_LIBRARY_FOUND)
    set(HAVE_SYMENGINE_MPC True)
else()
    set(HAVE_SYMENGINE_MPC False)
endif()

if(HAVE_SYMENGINE_MPC OR MPFR_LIBRARY_FOUND)
    set(HAVE_SYMENGINE_MPFR True)
else()
    set(HAVE_SYMENGINE_MPFR False)
endif()

message("Copying source of python wrappers into: ${CMAKE_CURRENT_BINARY_DIR}")
file(COPY symengine/ DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/symengine)

add_subdirectory(symengine)
