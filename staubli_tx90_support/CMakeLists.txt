cmake_minimum_required(VERSION 3.10)
project(staubli_tx90_support)

install(DIRECTORY config launch meshes urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
