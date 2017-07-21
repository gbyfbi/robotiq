cmake_minimum_required(VERSION 2.8.3)
project(robotiq_ethercat)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(soem_INCLUDE_DIRS "/home/gao/Downloads/SOEM/install/include")
set(CMAKE_VERBOSE_MAKEFILE on)
find_package(catkin REQUIRED COMPONENTS
#  soem
  roscpp
)

catkin_package(
   INCLUDE_DIRS include
   LIBRARIES robotiq_ethercat
#   CATKIN_DEPENDS soem roscpp
   CATKIN_DEPENDS roscpp
)

include_directories(include
  ${catkin_INCLUDE_DIRS}
  ## The following work around allows SOEM headers to include other SOEM headers.
  ## SOEM headers assume all headers are installed in a flat directory structure
  ## See https://github.com/smits/soem/issues/4 for more information.
#  ${soem_INCLUDE_DIRS}/soem
  ${soem_INCLUDE_DIRS}
)

add_library(robotiq_ethercat
 include/robotiq_ethercat/ethercat_manager.h
 src/ethercat_manager.cpp
)
target_link_libraries(robotiq_ethercat "/home/gao/Downloads/SOEM/install/lib/libsoem.a")
install(TARGETS robotiq_ethercat
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
 )

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)
