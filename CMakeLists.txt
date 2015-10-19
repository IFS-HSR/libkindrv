cmake_minimum_required (VERSION 2.8.8)
project (libkindrv)

set (libkindrv_INCLUDE_DIRS include/)
set (CMAKECONFIG_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/share/cmake/${PROJECT_NAME})

# The version number.
set (libkindrv_VERSION_MAJOR 0)
set (libkindrv_VERSION_MINOR 1)
set (libkindrv_VERSION_PATCH 0)
set (libkindrv_VERSION ${libkindrv_VERSION_MAJOR}.${libkindrv_VERSION_MINOR}.${libkindrv_VERSION_PATCH})

# find libusb library
set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake_modules/")
find_package (libusb-1.0 REQUIRED)

include_directories (
  include
  ${LIBUSB_1_INCLUDE_DIRS})

add_library (kindrv SHARED
  src/kindrv.cpp
  src/exception.cpp)
target_link_libraries (kindrv
  ${LIBUSB_1_LIBRARIES})

# Install headers.
install (DIRECTORY include/${PROJECT_NAME}
  DESTINATION ${CMAKE_INSTALL_PREFIX}/include)

# Install targets.
install (TARGETS kindrv
  DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
  EXPORT kindrv-targets)

# Install the package.xml file to be compatible with REP-136.
install (FILES package.xml
  DESTINATION share/${PROJECT_NAME})

# Generate and install a CMake configuration file.
include (CMakePackageConfigHelpers)
configure_package_config_file (cmake_modules/${PROJECT_NAME}Config.cmake.in
  ${PROJECT_NAME}Config.cmake
  INSTALL_DESTINATION ${CMAKECONFIG_INSTALL_DIR}
  PATH_VARS libkindrv_INCLUDE_DIRS)

#write_basic_package_version_file (
#  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
#  VERSION ${libkindrv_VERSION}
#  COMPATIBILITY SameMajorVersion)

install (EXPORT kindrv-targets
  FILE ${PROJECT_NAME}Targets.cmake
  DESTINATION ${CMAKECONFIG_INSTALL_DIR})

install (FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
              #${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
  DESTINATION ${CMAKECONFIG_INSTALL_DIR})


# Generate and install a pkg-config file.
#configure_file ("${CMAKE_SOURCE_DIR}/kindrv.pc.in" "${CMAKE_CURRENT_BINARY_DIR}/libkindrv.pc" @ONLY)
#install (FILES "${CMAKE_CURRENT_BINARY_DIR}/libkindrv.pc" DESTINATION lib/pkgconfig)
