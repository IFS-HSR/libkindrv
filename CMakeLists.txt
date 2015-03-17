if(DEFINED CATKIN_TOPLEVEL)
  cmake_minimum_required (VERSION 2.8.3)
else()
  cmake_minimum_required (VERSION 2.6)
endif()
project (libkindrv)

# The version number.
set (libkindrv_VERSION_MAJOR 0)
set (libkindrv_VERSION_MINOR 1)
set (libkindrv_VERSION_PATCH 0)
set (libkindrv_VERSION ${libkindrv_VERSION_MAJOR}.${libkindrv_VERSION_MINOR}.${libkindrv_VERSION_PATCH})

# Compile examples?
#option (BUILD_EXAMPLES "Build examples?" OFF)

# find libusb library
if(DEFINED CATKIN_TOPLEVEL)
  set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")
else()
  set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake_modules/")
endif()
find_package (libusb-1.0 REQUIRED)
include_directories (${LIBUSB_1_INCLUDE_DIRS})

if(DEFINED CATKIN_TOPLEVEL)
  catkin_package(
    INCLUDE_DIRS include
    LIBRARIES kindrv)
endif()

# add subdirectory containing source files
add_subdirectory (src)

if(DEFINED CATKIN_TOPLEVEL)
  include_directories(include)
endif()

# configure and install pkg-config file
if(DEFINED CATKIN_TOPLEVEL)
  configure_file ("${CMAKE_CURRENT_SOURCE_DIR}/kindrv.pc.in" "${CMAKE_CURRENT_BINARY_DIR}/libkindrv.pc" @ONLY)
else()
  configure_file ("${CMAKE_SOURCE_DIR}/kindrv.pc.in" "${CMAKE_CURRENT_BINARY_DIR}/libkindrv.pc" @ONLY)
endif()
install (FILES "${CMAKE_CURRENT_BINARY_DIR}/libkindrv.pc" DESTINATION lib/pkgconfig)
