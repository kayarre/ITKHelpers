cmake_minimum_required(VERSION 2.8)

add_library(libITKHelpers ITKHelpers.cpp)

# This is needed for shared_ptr and the trick using enable_if and if_fundamental to allow scalars
# to be treated as the 0th component of a vector.
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=gnu++0x")

if(BuildTests)
  add_subdirectory(Tests)
endif(BuildTests)