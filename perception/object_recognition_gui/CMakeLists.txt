# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(object_recognition_gui)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS roscpp actionlib actionlib_msgs rviz sensor_msgs stereo_msgs geometry_msgs shape_msgs interactive_perception_msgs)

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


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# build is where the UI file outputs (ui_*.h) go.
include_directories( build )

find_package(Qt4 COMPONENTS QtCore QtGui QtOpenGL REQUIRED)
include(${QT_USE_FILE})
add_definitions(-DQT_NO_KEYWORDS)

qt4_wrap_ui(UIC_FILES
  src/object_recognition_frame.ui
)

qt4_wrap_cpp(MOC_FILES
  include/object_recognition_gui/object_recognition_rviz_ui.h
  include/object_recognition_gui/mouse_event_signalling_render_panel.h
)

add_library(object_recognition_gui_rviz_plugin 
  src/init.cpp 
  src/object_recognition_display.cpp
  src/object_recognition_rviz_ui.cpp
  src/mouse_event_signalling_render_panel.cpp
  ${UIC_FILES}
  ${MOC_FILES}
  )

target_link_libraries(object_recognition_gui_rviz_plugin ${QT_LIBRARIES})

add_executable(ping_object_recognition_gui src/ping_object_recognition_gui.cpp)## Generate added messages and services with any dependencies listed here
#generate_messages(
#    #TODO DEPENDENCIES geometry_msgs std_msgs
#)

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS roscpp actionlib actionlib_msgs rviz sensor_msgs stereo_msgs geometry_msgs shape_msgs interactive_perception_msgs
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  PATTERN ".svn" EXCLUDE
)
