cmake_minimum_required(VERSION 3.1)
project(hive_explore)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  actionlib
  actionlib_msgs
  costmap_2d
  geometry_msgs
  map_msgs
  move_base_msgs
  nav_msgs
  roscpp
  rospy
  std_msgs
  tf
  visualization_msgs
  message_generation
)

add_service_files(
  FILES
  Map.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs
)

###################################
## catkin specific configuration ##
###################################
catkin_package(
  CATKIN_DEPENDS
    actionlib
    actionlib_msgs
    costmap_2d
    geometry_msgs
    map_msgs
    move_base_msgs
    nav_msgs
    roscpp
    std_msgs
    tf
    visualization_msgs
)

###########
## Build ##
###########
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

## Specify additional locations of header files
include_directories(
  ${catkin_INCLUDE_DIRS}
  include
)

add_executable(hive_explore
  src/costmap_client.cpp
  src/explore.cpp
  src/frontier_search.cpp
)
add_dependencies(hive_explore ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(hive_explore ${catkin_LIBRARIES})

#############
## Install ##
#############

# install nodes
install(TARGETS hive_explore
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

# install roslaunch files
install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)

#############
## Testing ##
#############
if(CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)

  # test all launch files
  roslaunch_add_file_check(launch)
endif()
