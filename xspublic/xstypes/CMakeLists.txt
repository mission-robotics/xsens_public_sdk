cmake_minimum_required(VERSION 3.15)
project(xstypes C CXX)

# ==============================================
# Targets

# xstypes
set( XSTYPES_SOURCES "" )
file(GLOB_RECURSE XSTYPES_SOURCES CONFIGURE_DEPENDS "*.c" "*.cpp" )

set( XSTYPES_PUBLIC_HEADERS "" )
file(GLOB_RECURSE XSTYPES_PUBLIC_HEADERS CONFIGURE_DEPENDS "*.h" )

add_library( xstypes )
target_sources( xstypes 
PRIVATE
  ${XSTYPES_SOURCES} 
)

target_include_directories(xstypes 
PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
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

# ==============================================
# Install

install( FILES ${XSTYPES_PUBLIC_HEADERS} DESTINATION include/xstypes )
install(
  TARGETS xstypes
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)