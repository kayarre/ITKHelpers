cmake_minimum_required(VERSION 3.0)

project(ITKHelpers)
SET(CMAKE_INCLUDE_CURRENT_DIR ON)
ENABLE_TESTING() # This has to be in the root directory if we want to run 'ctest' from the root build directory.

<<<<<<< HEAD
add_subdirectory(CMakeHelpers)
=======
cmake_minimum_required(VERSION 3.0)

# Only include Helpers if it is not already available
if(NOT TARGET Helpers)
  add_subdirectory(Helpers)
  include_directories(${Helpers_includes})
  set(ITKHelpers_includes ${ITKHelpers_includes} ${Helpers_includes})
  set(ITKHelpers_libraries ${ITKHelpers_libraries} Helpers)
  message("ITKHelpers submodule in ${CMAKE_CURRENT_SOURCE_DIR} used Helpers submodule (with includes ${Helpers_includes} ).")
endif()
>>>>>>> 126e2da

UseSubmodule(Helpers ITKHelpers)

# Enable c++11
if(UNIX)
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")
endif(UNIX)

# ITK
if(NOT ITK_FOUND)
  FIND_PACKAGE(ITK REQUIRED ITKCommon ITKIOImageBase ITKIOPNG
                            ITKImageIntensity ITKImageFeature ITKMathematicalMorphology
                            ITKBinaryMathematicalMorphology)
  INCLUDE(${ITK_USE_FILE})
endif()
set(ITKHelpers_libraries ${ITKHelpers_libraries} ${ITK_LIBRARIES})

# Give the compiler all of the required include directories
include_directories(${ITKHelpers_include_dirs})

# Create the library
add_library(ITKHelpers ITKHelpers.cpp)
target_link_libraries(ITKHelpers ${ITKHelpers_libraries})
set(ITKHelpers_libraries ${ITKHelpers_libraries} ITKHelpers)

# Add all of the non-compiled files to the project
add_custom_target(ITKHelpersSources SOURCES
ITKHelpers.hpp ITKTypeTraits.h ITKContainerInterface.h ITKContainerInterface.hpp
ITKHelpersTypes.h itkRGBToHSVColorSpacePixelAccessor.h itkRGBToLabColorSpacePixelAccessor.h)

# Allow this project to be detected and used as a submodule
CreateSubmodule(ITKHelpers)

# Build the tests if requested
option(ITKHelpers_BuildTests "Build ITKHelpers tests?" OFF)
if(ITKHelpers_BuildTests)
  add_subdirectory(Tests)
endif()
