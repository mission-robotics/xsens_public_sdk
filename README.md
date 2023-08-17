This is the modified release of the Xsens Public SDK, obtained from release:

MT_Software_Suite_linux-x64_2022.0_b7085_r119802.tar.gz

This release converts the original makefile-based project to a CMake library and fixes several aspects of the code that introduce compiler warnings and errors, as a result of using newer toolchains and enabling our standard set of warning/error compile flags.

### Requirements
The only external dependency for this library is libusb. When building standalone, this can be installed via:
```bash
sudo apt update
sudo apt install libusb-dev
```
When building within the context of a ROS2 environment, the dependency is included in `package.xml` and should be installed via `rosdep`.

### Building and running the libraries and example app
```bash
mkdir build
cd build
cmake .. -DBUILD_EXAMPLES=1
make -j

./build/mti_recv
```

### Building with colcon
```bash
source /opt/ros/iron/setup.bash 
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release -DBUILD_EXAMPLES=1

./install/xsens_public_sdk/bin/mti_recv
```