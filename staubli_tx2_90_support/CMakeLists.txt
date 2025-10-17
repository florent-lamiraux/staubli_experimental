# Copyright 2025 CNRS
# Author: Florent Lamiraux
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:

# 1. Redistributions of source code must retain the above copyright
# notice, this list of conditions and the following disclaimer.

# 2. Redistributions in binary form must reproduce the above copyright
# notice, this list of conditions and the following disclaimer in the
# documentation and/or other materials provided with the distribution.

# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

cmake_minimum_required(VERSION 3.22)
set(CXX_DISABLE_WERROR TRUE)
set(DOXYGEN_USE_TEMPLATE_CSS TRUE)
set(PROJECT_USE_CMAKE_EXPORT TRUE)

set(PROJECT_NAME staubli_tx2_90_support)
set(PROJECT_DESCRIPTION
    "Model of the Staubli TX2 90 robot")
set(PROJECT_URL "https://https://github.com/florent-lamiraux/staubli_experimental")

find_package(jrl-cmakemodules QUIET CONFIG)
if(jrl-cmakemodules_FOUND)
  get_property(
    JRL_CMAKE_MODULES
    TARGET jrl-cmakemodules::jrl-cmakemodules
    PROPERTY INTERFACE_INCLUDE_DIRECTORIES)
  message(STATUS "JRL cmakemodules found on system at ${JRL_CMAKE_MODULES}")
else()
  message(STATUS "JRL cmakemodules not found. Let's fetch it.")
  include(FetchContent)
  FetchContent_Declare(
    "jrl-cmakemodules"
    GIT_REPOSITORY "https://github.com/jrl-umi3218/jrl-cmakemodules.git")
  FetchContent_MakeAvailable("jrl-cmakemodules")
  FetchContent_GetProperties("jrl-cmakemodules" SOURCE_DIR JRL_CMAKE_MODULES)
endif()

include("${JRL_CMAKE_MODULES}/base.cmake")

compute_project_args(PROJECT_ARGS)
project(${PROJECT_NAME} ${PROJECT_ARGS})

# Create an interface library for cmake to install targets.
set(LIBRARY_NAME ${PROJECT_NAME})
add_library(${LIBRARY_NAME} INTERFACE)
install(TARGETS ${LIBRARY_NAME}
  EXPORT ${TARGETS_EXPORT_NAME}
  DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
  )

# set(CMAKE_INSTALL_DATAROOTDIR ${CMAKE_INSTALL_PREFIX}/share)
install(DIRECTORY config launch meshes urdf
  DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME})
install(FILES package.xml
  DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME})



# The file below is necessary for xacro to find the installed xacro files
file(
  WRITE
  ${CMAKE_CURRENT_BINARY_DIR}/share/ament_index/resource_index/packages/${PROJECT_NAME}
  "")
install(
  FILES
    ${CMAKE_CURRENT_BINARY_DIR}/share/ament_index/resource_index/packages/${PROJECT_NAME}
  DESTINATION share/ament_index/resource_index/packages)

