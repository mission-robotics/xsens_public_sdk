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

# Create a pseudo-target that other targets (i.e. examples, tests) can depend
# on and can also be provided as import-target by a package-file when building
# those targets outside the regular build-tree (i.e. the installed tree)
add_library(${PROJECT_NAME}::xstypes ALIAS xstypes)

# ==============================================
# Install

# Headers
install( 
  FILES ${XSTYPES_PUBLIC_HEADERS} 
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/xstypes
  COMPONENT dev )

# Binaries
install(
  TARGETS xstypes
  EXPORT "${PROJECT_NAME}"
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT lib
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
)