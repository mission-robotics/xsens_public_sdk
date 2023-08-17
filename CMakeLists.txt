cmake_minimum_required(VERSION 3.15)
project(xsens_public_sdk C CXX)

# Libraries
add_subdirectory(xspublic)

# Examples
if( BUILD_EXAMPLES )
  add_subdirectory( examples )
endif()