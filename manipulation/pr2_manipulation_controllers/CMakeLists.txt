# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(pr2_manipulation_controllers)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS pluginlib tf geometry_msgs robot_mechanism_controllers std_srvs rospy roscpp rosrt angles tf_conversions std_msgs diagnostic_msgs pr2_controller_interface pr2_controller_manager kdl_parser pr2_mechanism_model control_toolbox realtime_tools eigen_conversions rosatomic allocators lockfree pr2_wrappers pr2_mechanism_controllers)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
add_message_files(
  FILES
  # TODO: List your msg files here
    JinvExperimentalControllerState.msg
    JinvTeleopControllerState.msg
    JTTaskControllerState.msg
    JTTeleopControllerState.msg
)
#uncomment if you have defined services
add_service_files(
  FILES
  # TODO: List your msg files here
)

find_package(Eigen REQUIRED)
include_directories(SYSTEM ${Eigen_INCLUDE_DIRS})
add_definitions(${EIGEN_DEFINITIONS})

add_library(pr2_manipulation_controllers
  src/cartesian_trajectory_controller.cpp
  src/jinv_teleop_controller.cpp
  src/jt_task_controller.cpp
  src/jinv_experimental_controller.cpp
)


#common commands for building c++ executables and libraries
#add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#
# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()
#find_package(Boost REQUIRED COMPONENTS thread)
include_directories(${Boost_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})
#add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})
## Generate added messages and services with any dependencies listed here
generate_messages(
    DEPENDENCIES geometry_msgs std_msgs
)

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS pluginlib kdl tf geometry_msgs robot_mechanism_controllers std_srvs rospy roscpp rosrt angles tf_conversions std_msgs diagnostic_msgs object_manipulation_msgs pr2_controller_interface pr2_controller_manager kdl_parser pr2_mechanism_model control_toolbox realtime_tools eigen_conversions rosatomic allocators lockfree pr2_wrappers pr2_mechanism_controllers common_rosdeps
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)

install(FILES controller_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/
  )

install(DIRECTORY include/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}/
  PATTERN ".svn" EXCLUDE)
