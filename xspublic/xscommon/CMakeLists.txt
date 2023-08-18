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

# Create a pseudo-target that other targets (i.e. examples, tests) can depend
# on and can also be provided as import-target by a package-file when building
# those targets outside the regular build-tree (i.e. the installed tree)
add_library(${PROJECT_NAME}::xscommon ALIAS xscommon)

# ==============================================
# Install

# Headers
install( 
  FILES ${XSCOMMON_PUBLIC_HEADERS} 
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/xscommon
  COMPONENT dev )

# Binaries
install(
  TARGETS xscommon
  EXPORT "${PROJECT_NAME}"
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT lib
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
  ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT lib
)