# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(pr2_gripper_sensor_action)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS roscpp actionlib_msgs actionlib pr2_gripper_sensor_msgs pr2_mechanism_controllers robot_mechanism_controllers pr2_controllers_msgs pr2_mechanism_model pr2_gripper_sensor_controller pr2_machine message_generation)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)



# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#add_message_files(
#uncomment if you have defined services
#add_service_files(
include_directories(${catkin_INCLUDE_DIRS})

link_directories(${catkin_INCLUDE_DIRS})

add_executable(pr2_gripper_sensor_action src/pr2_gripper_sensor_action.cpp)
add_dependencies(pr2_gripper_sensor_action ${catkin_EXPORTED_TARGETS})
target_link_libraries(pr2_gripper_sensor_action ${catkin_LIBRARIES})

add_executable(pr2_gripper_findContact_action src/pr2_gripper_findContact_action.cpp)
target_link_libraries(pr2_gripper_findContact_action ${catkin_LIBRARIES})
add_dependencies(pr2_gripper_findContact_action ${catkin_EXPORTED_TARGETS})

add_executable(pr2_gripper_slipServo_action src/pr2_gripper_slipServo_action.cpp)
target_link_libraries(pr2_gripper_slipServo_action ${catkin_LIBRARIES})
add_dependencies(pr2_gripper_slipServo_action ${catkin_EXPORTED_TARGETS})

add_executable(pr2_gripper_event_detector_action     src/pr2_gripper_event_detector_action.cpp)
target_link_libraries(pr2_gripper_event_detector_action ${catkin_LIBRARIES})
add_dependencies(pr2_gripper_event_detector_action ${catkin_EXPORTED_TARGETS})

add_executable(pr2_gripper_forceServo_action     src/pr2_gripper_forceServo_action.cpp)
target_link_libraries(pr2_gripper_forceServo_action ${catkin_LIBRARIES})
add_dependencies(pr2_gripper_forceServo_action pr2_gripper_sensor_msgs_gencpp)

add_executable(positionActionTest              src/positionActionTest.cpp)
target_link_libraries(positionActionTest ${catkin_LIBRARIES})

add_executable(findContactActionTest          src/findContactActionTest.cpp)
add_dependencies(findContactActionTest ${catkin_EXPORTED_TARGETS})
target_link_libraries(findContactActionTest ${catkin_LIBRARIES})

add_executable(slipServoActionTest          src/slipServoActionTest.cpp)
target_link_libraries(slipServoActionTest ${catkin_LIBRARIES})
add_dependencies(slipServoActionTest ${catkin_EXPORTED_TARGETS})

add_executable(placeActionTest          src/placeActionTest.cpp)
target_link_libraries(placeActionTest ${catkin_LIBRARIES})
add_dependencies(placeActionTest ${catkin_EXPORTED_TARGETS})

add_executable(forceServoActionTest          src/forceServoActionTest.cpp)
target_link_libraries(forceServoActionTest ${catkin_LIBRARIES})
add_dependencies(forceServoActionTest ${catkin_EXPORTED_TARGETS})

add_executable(pr2_gripper_grab_action          src/pr2_gripper_grab_action.cpp)
target_link_libraries(pr2_gripper_grab_action ${catkin_LIBRARIES})
add_dependencies(pr2_gripper_grab_action ${catkin_EXPORTED_TARGETS})

add_executable(pr2_gripper_release_action          src/pr2_gripper_release_action.cpp)
target_link_libraries(pr2_gripper_release_action ${catkin_LIBRARIES})
add_dependencies(pr2_gripper_release_action ${catkin_EXPORTED_TARGETS})

add_executable(grabActionTest          src/grabActionTest.cpp)
target_link_libraries(grabActionTest ${catkin_LIBRARIES})
add_dependencies(grabActionTest ${catkin_EXPORTED_TARGETS})

#common commands for building c++ executables and libraries
#add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#
# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()
#find_package(Boost REQUIRED COMPONENTS thread)
include_directories(${Boost_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${catkin_LIBRARIES})
#add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})
## Generate added messages and services with any dependencies listed here

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS roscpp actionlib_msgs actionlib pr2_gripper_sensor_msgs pr2_mechanism_controllers robot_mechanism_controllers pr2_controllers_msgs pr2_mechanism_model pr2_gripper_sensor_controller pr2_machine message_runtime
    CATKIN_DEPENDS  # TODO
    INCLUDE_DIRS  ${catkin_INCLUDE_DIRS}# TODO include
    LIBRARIES # TODO
)

