# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(tabletop_object_detector)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS roscpp tf visualization_msgs sensor_msgs geometry_msgs shape_msgs pcl_ros household_objects_database_msgs)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
set(ROS_BUILD_TYPE Debug)


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

find_package(Eigen REQUIRED)
include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS})

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
add_message_files(
  FILES
  # TODO: List your msg files here
)
#uncomment if you have defined services
add_service_files(
  FILES
  # TODO: List your msg files here
)

#common commands for building c++ executables and libraries
#add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#
# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()
#find_package(Boost REQUIRED COMPONENTS system thread)
include_directories(${Boost_INCLUDE_DIRS})
#add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

add_library(marker_generator src/marker_generator.cpp )

add_executable(tabletop_segmentation src/tabletop_segmentation.cpp )
target_link_libraries(tabletop_segmentation marker_generator)		    

add_library(tabletop_model_fitter src/model_fitter.cpp 
					   src/iterative_distance_fitter.cpp)

add_executable(tabletop_object_recognition src/tabletop_object_recognition.cpp 
                                                    src/marker_generator.cpp)
target_link_libraries(tabletop_object_recognition tabletop_model_fitter)

# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()
target_link_libraries(tabletop_object_recognition ${Boost_LIBRARIES})

add_executable(tabletop_complete_node src/tabletop_complete_node.cpp)

add_executable(ping_tabletop_node src/ping_tabletop_node.cpp)

add_executable(publish_database_object src/publish_database_object.cpp)

add_executable(segment_object_in_hand src/segment_object_in_hand.cpp)

add_executable(ping_segment_object_in_hand src/ping_segment_object_in_hand.cpp)

## Generate added messages and services with any dependencies listed here

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS roscpp tf visualization_msgs sensor_msgs geometry_msgs shape_msgs pcl_ros distance_field household_objects_database_msgs
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)
