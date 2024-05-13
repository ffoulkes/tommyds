# Copyright 2021-2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.15)

project(tommyds VERSION 3.0 LANGUAGES C)

#-----------------------------------------------------------------------
# Modules
#-----------------------------------------------------------------------
include(CMakePackageConfigHelpers)
include(CMakePrintHelpers)
include(GNUInstallDirs)

#-----------------------------------------------------------------------
# Build
#-----------------------------------------------------------------------
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_subdirectory(tommyds)

add_library(tommyds SHARED
  $<TARGET_OBJECTS:tommyds_o>
)

target_include_directories(tommyds PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/tommyds>
  $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${CMAKE_INSTALL_INCLUDEDIR}>
)

set_target_properties(tommyds PROPERTIES
  VERSION ${PROJECT_VERSION}
  SOVERSION ${PROJECT_VERSION_MAJOR}
)

#-----------------------------------------------------------------------
# Install
#-----------------------------------------------------------------------
# EXPORT parameter value
set(EXPORTNAME TommyDS)

# cmake config file prefix
set(EXPORTSTEM tommyds)

install(
  TARGETS tommyds
  EXPORT ${EXPORTNAME}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
    DIRECTORY tommyds
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING
      PATTERN "*.h"
)

#-----------------------------------------------------------------------
# pkg-config file
#-----------------------------------------------------------------------
set(_pkgconfig_file ${CMAKE_CURRENT_BINARY_DIR}/pkgconfig/libtommyds.pc)

configure_file(cmake/libtommyds.pc.in ${_pkgconfig_file} @ONLY)

install(
  FILES ${_pkgconfig_file}
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

#-----------------------------------------------------------------------
# CMake configuration file
#-----------------------------------------------------------------------
install(EXPORT ${EXPORTNAME}
  FILE ${EXPORTSTEM}-targets.cmake
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORTSTEM}
)

configure_package_config_file(
  cmake/${EXPORTSTEM}-config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${EXPORTSTEM}-config.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORTSTEM}
)

install(
  FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${EXPORTSTEM}-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${EXPORTSTEM}-config-version.cmake
  DESTINATION
    ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORTSTEM}
)

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/${EXPORTSTEM}-config-version.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion
)
