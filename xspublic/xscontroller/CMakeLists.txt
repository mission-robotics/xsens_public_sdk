# ==============================================
# Dependencies

find_package(PkgConfig REQUIRED)
pkg_search_module(LIBUSB REQUIRED libusb)

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
  ${LIBUSB_INCLUDE_DIRS}
)

target_link_directories(xscontroller 
PUBLIC 
  ${LIBUSB_LIBRARY_DIRS} 
)

target_link_libraries(xscontroller
PUBLIC
    xscommon
    ${LIBUSB_LIBRARIES}
)

target_compile_options(xscontroller 
PUBLIC
  -Wall 
  -Wextra 
  -Wpedantic
  -Wno-unused-function
)

target_compile_definitions(xscontroller
PUBLIC
    -DHAVE_LIBUSB
)

target_compile_features(xscontroller
PUBLIC
  c_std_11
  cxx_std_17
)

# Create a pseudo-target that other targets (i.e. examples, tests) can depend
# on and can also be provided as import-target by a package-file when building
# those targets outside the regular build-tree (i.e. the installed tree)
add_library(${PROJECT_NAME}::xscontroller ALIAS xscontroller)

# ==============================================
# Install

# Headers
install( 
  FILES ${XSCONTROLLER_PUBLIC_HEADERS} 
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/xscontroller
  COMPONENT dev )

# Binaries
install(
  TARGETS xscontroller
  EXPORT "${PROJECT_NAME}"
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT lib
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
)