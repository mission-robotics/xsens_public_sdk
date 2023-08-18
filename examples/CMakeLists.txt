cmake_minimum_required(VERSION 3.15)
project(xsens_examples C CXX)

# ==============================================
# Targets

add_executable( mti_recv
  "example_mti_receive_data.cpp"
)

target_link_libraries( mti_recv
PUBLIC
  xscontroller
)

target_compile_options( mti_recv
PUBLIC
  -Wall 
  -Wextra 
  -Wpedantic
)

target_compile_features( mti_recv
PUBLIC
  cxx_std_17
)

# ==============================================
# Install

install(
  TARGETS mti_recv
  RUNTIME DESTINATION bin
)