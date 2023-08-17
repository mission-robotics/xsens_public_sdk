cmake_minimum_required(VERSION 3.15)
project(xscontroller C CXX)

# ==============================================
# Targets

# xscontroller
set( XSCONTROLLER_SOURCES "" )
file(GLOB_RECURSE XSCONTROLLER_SOURCES CONFIGURE_DEPENDS "*.c" "*.cpp" )

set( XSCONTROLLER_PUBLIC_HEADERS "" )
file(GLOB_RECURSE XSCONTROLLER_PUBLIC_HEADERS CONFIGURE_DEPENDS "*.h" )

add_library( xscontroller )
target_sources( xscontroller 
PRIVATE
  ${XSCONTROLLER_SOURCES} 
)

target_include_directories(xscontroller 
PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
  $<INSTALL_INTERFACE:include>
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

install( FILES ${XSCONTROLLER_PUBLIC_HEADERS} DESTINATION include/xscontroller )
install(
  TARGETS xscontroller
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)