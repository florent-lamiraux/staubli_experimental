cmake_minimum_required(VERSION 3.10)
project(staubli_tx2_90_support)

set(CMAKE_INSTALL_DATAROOTDIR ${CMAKE_INSTALL_PREFIX}/share)
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

