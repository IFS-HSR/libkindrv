cmake_minimum_required (VERSION 2.8.8)
project (libkindrv)

set (libkindrv_INCLUDE_DIRS include/)
set (CONFIG_PACKAGE_LOCATION lib/${PROJECT_NAME})

# The version number.
set (libkindrv_VERSION_MAJOR 0)
set (libkindrv_VERSION_MINOR 1)
set (libkindrv_VERSION_PATCH 0)
set (libkindrv_VERSION ${libkindrv_VERSION_MAJOR}.${libkindrv_VERSION_MINOR}.${libkindrv_VERSION_PATCH})
set_property(TARGET ${PROJECT_NAME} PROPERTY VERSION ${libkindrv_VERSION})

# find libusb library
set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake_modules/")
find_package (libusb-1.0 REQUIRED)

include_directories (
  include
  ${LIBUSB_1_INCLUDE_DIRS})

add_library (${PROJECT_NAME} SHARED
  src/kindrv.cpp
  src/exception.cpp)

target_link_libraries (${PROJECT_NAME}
  ${LIBUSB_1_LIBRARIES})

# Install headers.
install (DIRECTORY include/${PROJECT_NAME}
  DESTINATION include
)

# Install targets.
install (TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}Targets
  LIBRARY DESTINATION lib/${PROJECT_NAME}
  INCLUDES DESTINATION include/${PROJECT_NAME}
)

# Install the package.xml file to be compatible with REP-136.
install (FILES package.xml
  DESTINATION share/${PROJECT_NAME}
)

# Generate and install a CMake configuration file.
include (CMakePackageConfigHelpers)
write_basic_package_version_file (
 ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}/${PROJECT_NAME}ConfigVersion.cmake
 VERSION ${libkindrv_VERSION}
 COMPATIBILITY SameMajorVersion
)

export (TARGETS ${PROJECT_NAME}
  FILE "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}/${PROJECT_NAME}Targets.cmake"
)
configure_file (cmake_modules/${PROJECT_NAME}Config.cmake
  "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}/${PROJECT_NAME}Config.cmake"
  COPYONLY
)

install (EXPORT ${PROJECT_NAME}Targets
  FILE ${PROJECT_NAME}Targets.cmake
  DESTINATION ${CONFIG_PACKAGE_LOCATION}
)
install (
  FILES
    cmake_modules/${PROJECT_NAME}Config.cmake
    "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}/${PROJECT_NAME}ConfigVersion.cmake"
  DESTINATION ${CONFIG_PACKAGE_LOCATION}
)
