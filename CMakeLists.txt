cmake_minimum_required(VERSION 3.15)
project(xsens_example C CXX)


## Deps
add_subdirectory(xspublic)

# ==============================================
# Targets

add_executable( xsens_example
  "example_mti_receive_data.cpp"
)

target_link_libraries( xsens_example
PUBLIC
  xscontroller
  xscommon
  xstypes
)

target_compile_options(${PROJECT_NAME}
PUBLIC
  -Wall 
  -Wextra 
  -Wpedantic
)

target_compile_features(${PROJECT_NAME}
PUBLIC
  cxx_std_17
)

# ==============================================
# Install

install(
  DIRECTORY include/
  DESTINATION include
)
install(
  TARGETS xsens_example
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)