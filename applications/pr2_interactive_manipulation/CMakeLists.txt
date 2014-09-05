# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(pr2_interactive_manipulation)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS tf  pr2_controllers_msgs sensor_msgs std_msgs std_srvs pr2_wrappers robot_self_filter_color move_base_msgs joy  joint_trajectory_generator map_server image_rotate   pr2_teleop topic_tools pr2_tuck_arms_action pr2_marker_control pr2_object_manipulation_msgs pr2_interactive_gripper_pose_action pr2_object_manipulation_launch dynamic_reconfigure pr2_plugs_actions)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# CMake 2.4.6 doesn't include FindPkgConfig, so we provide our own copy

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/FindPkgConfig.cmake)

find_package(Eigen REQUIRED)
include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS})

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
set(ROS_BUILD_TYPE RelWithDebInfo)


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# Dynamic reconfigure


# backend node
add_executable(interactive_manipulation_backend_node 
  src/interactive_manipulation_backend.cpp
  src/interactive_manipulation_backend_node.cpp
)

# user interface based on interactive markers
add_executable(interactive_marker_node
  src/interactive_marker_node.cpp
  src/graspable_object_handler.cpp
)

## Generate added messages and services with any dependencies listed here
#generate_messages(
#    #TODO DEPENDENCIES geometry_msgs std_msgs
#)

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS tf tabletop_collision_map_processing object_manipulation_msgs pr2_controllers_msgs sensor_msgs std_msgs std_srvs pr2_create_object_model pr2_wrappers robot_self_filter_color move_base_msgs segmented_clutter_grasp_planner tf_throttle pr2_interactive_object_detection joy pr2_2dnav joint_trajectory_generator map_server image_rotate point_cloud_server pr2_2dnav_local pr2_teleop topic_tools pr2_2dnav_slam pr2_tuck_arms_action pr2_marker_control pr2_object_manipulation_msgs pr2_interactive_gripper_pose_action object_manipulator pr2_object_manipulation_launch dynamic_reconfigure pr2_plugs_actions
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  PATTERN ".svn" EXCLUDE)

install(DIRECTORY docs/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/docs
  PATTERN ".svn" EXCLUDE)

install(DIRECTORY cfg/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/cfg
  PATTERN ".svn" EXCLUDE)

install(DIRECTORY include/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}/
  PATTERN ".svn" EXCLUDE)
