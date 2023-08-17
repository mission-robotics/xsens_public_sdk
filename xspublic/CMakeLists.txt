cmake_minimum_required(VERSION 3.15)
project(xsens_public_sdk C CXX)

# ==============================================
# Targets


# xstypes
set( XSTYPES_SOURCES "" )
file(GLOB_RECURSE XSTYPES_SOURCES CONFIGURE_DEPENDS "xstypes/*.c" "xstypes/*.cpp" )

add_library( xstypes )
target_sources( xstypes 
PRIVATE
  ${XSTYPES_SOURCES} 
)

target_include_directories(xstypes 
PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:include>
)

target_compile_options(xstypes 
PUBLIC
  -Wall 
  -Wextra 
  -Wpedantic
  -Wno-address-of-packed-member
  -Wno-unused-function
)

target_compile_features(xstypes
PUBLIC
  c_std_11
  cxx_std_17
)

# xscommon
set( XSCOMMON_SOURCES "" )
file(GLOB_RECURSE XSCOMMON_SOURCES CONFIGURE_DEPENDS "xscommon/*.c" "xscommon/*.cpp" )

add_library( xscommon )
target_sources( xscommon 
PRIVATE
  ${XSCOMMON_SOURCES} 
)

target_include_directories(xscommon 
PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/xscommon>
  $<INSTALL_INTERFACE:xscommon>
)

target_link_libraries(xscommon
PUBLIC
    xstypes
)

target_compile_options(xscommon 
PUBLIC
  -Wall 
  -Wextra 
  -Wpedantic
  -Wno-address-of-packed-member
)

target_compile_features(xscommon
PUBLIC
  c_std_11
  cxx_std_17
)

# xscontroller
set( XSCONTROLLER_SOURCES "" )
file(GLOB_RECURSE XSCONTROLLER_SOURCES CONFIGURE_DEPENDS "xscontroller/*.c" "xscontroller/*.cpp" )

add_library( xscontroller )
target_sources( xscontroller 
PRIVATE
  ${XSCONTROLLER_SOURCES} 
)

target_include_directories(xscontroller 
PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/xscontroller>
  $<INSTALL_INTERFACE:xscontroller>
)

target_link_libraries(xscontroller
PUBLIC
    xscommon
)

target_compile_options(xscontroller 
PUBLIC
  -Wall 
  -Wextra 
  -Wpedantic
  -Wno-address-of-packed-member
)

target_compile_definitions(xscontroller
PUBLIC
    -DHAVE_JOURNALLER
    -DHAVE_LIBUSB
)

target_compile_features(xscontroller
PUBLIC
  c_std_11
  cxx_std_17
)


# ==============================================
# Install

install(
  DIRECTORY include/
  DESTINATION include
)
install(
  TARGETS xstypes xscommon xscontroller
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)