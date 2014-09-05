# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(robot_self_filter_color)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS roscpp tf filters sensor_msgs visualization_msgs geometric_shapes urdf resource_retriever shape_msgs pcl_ros)

# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
find_package(Eigen REQUIRED)
find_package(PCL REQUIRED)
include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS}
                           ${PCL_INCLUDE_DIRS}
)

set(ROS_BUILD_TYPE Release)

find_package(PCL)
include_directories(SYSTEM ${PCL_INCLUDE_DIRS})
#find_package(PkgConfig REQUIRED)
#find_package(ASSIMP QUIET)
#if (NOT ASSIMP_FOUND)
#  pkg_check_modules(ASSIMP assimp)
#endif()
#if (ASSIMP_FOUND)
#  if( ${ASSIMP_VERSION} STRGREATER "2.0.0" )
#    set(IS_ASSIMP3 1)
#    add_definitions(-DIS_ASSIMP3)
#    message(STATUS "Found assimp v3")
#  else()
#    message(STATUS "Found assimp v2")
#  endif()
#  include_directories(${ASSIMP_INCLUDE_DIRS})
#  link_directories(${ASSIMP_LIBRARY_DIRS})
#else()
#  message(STATUS "could not find assimp (perhaps available #thorugh ROS package?), so assimping assimp v2")
#  set(ASSIMP_LIBRARIES assimp)
#  set(ASSIMP_LIBRARY_DIRS)
#  set(ASSIMP_CXX_FLAGS)
#  set(ASSIMP_CFLAGS_OTHER)
#  set(ASSIMP_LINK_FLAGS)
#  set(ASSIMP_INCLUDE_DIRS)
#  set(IS_ASSIMP3 0) # most likely not
#endif()


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()

add_library(robot_self_filter_color src/self_mask_color.cpp)
target_link_libraries(robot_self_filter_color assimp
                                        geometric_shapes
                                        ${PCL_LIBRARIES}
)

add_executable (test_filter_color src/test_filter_color.cpp)
target_link_libraries (test_filter_color robot_self_filter_color assimp)

add_executable(self_filter_color src/self_filter_color.cpp)
target_link_libraries(self_filter_color assimp
                                  geometric_shapes
                                  robot_self_filter_color
)
find_package(Boost REQUIRED COMPONENTS signals)
include_directories(${Boost_INCLUDE_DIRS})
target_link_libraries(self_filter_color ${Boost_LIBRARIES})
## Generate added messages and services with any dependencies listed here
#generate_messages(
#    #TODO DEPENDENCIES geometry_msgs std_msgs
#)

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS roscpp tf filters sensor_msgs visualization_msgs geometric_shapes urdf resource_retriever shape_msgs pcl assimp pkg-config
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)
