cmake_minimum_required(VERSION 3.15)
project(xscommon C CXX)

# ==============================================
# Targets

# xscommon
set( XSCOMMON_SOURCES "" )
file(GLOB_RECURSE XSCOMMON_SOURCES CONFIGURE_DEPENDS "*.c" "*.cpp" )

set( XSCOMMON_PUBLIC_HEADERS "" )
file(GLOB_RECURSE XSCOMMON_PUBLIC_HEADERS CONFIGURE_DEPENDS "*.h" )

add_library( xscommon )
target_sources( xscommon 
PRIVATE
  ${XSCOMMON_SOURCES} 
)

target_include_directories(xscommon 
PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
  $<INSTALL_INTERFACE:include>
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
  -Wno-unused-function
)

target_compile_features(xscommon
PUBLIC
  c_std_11
  cxx_std_17
)

# ==============================================
# Install

install( FILES ${XSCOMMON_PUBLIC_HEADERS} DESTINATION include/xscommon )
install(
  TARGETS xscommon
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)