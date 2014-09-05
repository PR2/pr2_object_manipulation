# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(pr2_interactive_object_detection)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS roscpp tf tabletop_object_detector  household_objects_database_msgs  sensor_msgs stereo_msgs actionlib_msgs rgbd_assembler tabletop_collision_map_processing interactive_perception_msgs)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
# CMake 2.4.6 doesn't include FindPkgConfig, so we provide our own copy

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/FindPkgConfig.cmake)


# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

#generate the actions


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


add_action_files(
  FILES
  UserCommand.action
)

###backend node

add_executable(pr2_interactive_object_detection_backend src/pr2_interactive_object_detection_backend.cpp)

## Generate added messages and services with any dependencies listed here

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS roscpp tf tabletop_object_detector household_objects_database household_objects_database_msgs object_manipulator sensor_msgs stereo_msgs actionlib_msgs rgbd_assembler tabletop_collision_map_processing interactive_perception_msgs object_recognition_core
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)

install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}/launch
  PATTERN ".svn" EXCLUDE
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  PATTERN ".svn" EXCLUDE
)
