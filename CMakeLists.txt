cmake_minimum_required(VERSION 3.15)
project(xsens_public_sdk VERSION 0.0.1 LANGUAGES C CXX)

# Set a default build type if none was specified
set(default_build_type "RelWithDebInfo")
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# Packaging
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

set(CMAKE_INSTALL_RPATH "$ORIGIN/../${CMAKE_INSTALL_LIBDIR}")
set(CMAKE_INSTALL_CMAKEDIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")

# Generate <Package>Config.cmake
configure_package_config_file(
  "PackageConfig.cmake.in"
  "${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION "${CMAKE_INSTALL_CMAKEDIR}")

# Generate <Package>Version.cmake
write_basic_package_version_file(
  "${PROJECT_NAME}ConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion)

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
  DESTINATION "${CMAKE_INSTALL_CMAKEDIR}"
  COMPONENT dev)

# Generate <Package>Targets.cmake
install(
  EXPORT ${PROJECT_NAME}
  FILE "${PROJECT_NAME}Targets.cmake"
  NAMESPACE "${PROJECT_NAME}::"
  DESTINATION "${CMAKE_INSTALL_CMAKEDIR}"
  COMPONENT dev)

# Libraries
add_subdirectory(xspublic)

# Examples
if( BUILD_EXAMPLES )
  add_subdirectory( examples )
endif()