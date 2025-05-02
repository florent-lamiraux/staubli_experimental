cmake_minimum_required(VERSION 3.10)
set(CXX_DISABLE_WERROR TRUE)
set(DOXYGEN_USE_TEMPLATE_CSS TRUE)
set(PROJECT_USE_CMAKE_EXPORT TRUE)

set(PROJECT_NAME staubli_tx2_90_support)
set(PROJECT_DESCRIPTION
    "Model of the Staubli TX2 90 robot")

# Find jrl-cmakemodules
find_package(jrl-cmakemodules QUIET CONFIG)
if(jrl-cmakemodules_FOUND)
  get_property(
    JRL_CMAKE_MODULES
    TARGET jrl-cmakemodules::jrl-cmakemodules
    PROPERTY INTERFACE_INCLUDE_DIRECTORIES)
  message(STATUS "JRL cmakemodules found on system at ${JRL_CMAKE_MODULES}")
elseif(${CMAKE_VERSION} VERSION_LESS "3.14.0")
  message(
    FATAL_ERROR
      "\nCan't find jrl-cmakemodules. Please either:\n"
      "  - use git submodule: 'git submodule update --init'\n"
      "  - or install https://github.com/jrl-umi3218/jrl-cmakemodules\n"
      "  - or upgrade your CMake version to >= 3.14 to allow automatic fetching\n"
  )
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

add_library(${PROJECT_NAME} INTERFACE)
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

