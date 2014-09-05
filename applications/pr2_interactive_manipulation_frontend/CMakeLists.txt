# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(pr2_interactive_manipulation_frontend)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS rviz tf pr2_interactive_object_detection_frontend object_recognition_gui pr2_controllers_msgs pr2_object_manipulation_msgs sensor_msgs std_msgs std_srvs pr2_msgs pr2_object_manipulation_msgs)

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

# This will disable lots of warnings from OgreException.h
set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-unused-but-set-parameter" )

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# build is where the UI file outputs (ui_*.h) go.
include_directories( build )

pkg_check_modules(OGRE OGRE)
include_directories(SYSTEM ${OGRE_INCLUDE_DIRS} )
link_directories( ${OGRE_LIBRARY_DIRS} )

find_package(Qt4 COMPONENTS QtCore QtGui QtOpenGL REQUIRED)
include(${QT_USE_FILE})
add_definitions(-DQT_NO_KEYWORDS)

qt4_wrap_ui(UIC_FILES
  src/advanced_options_dialog.ui
  src/interactive_manipulation.ui
)

qt4_wrap_cpp(MOC_FILES
  include/pr2_interactive_manipulation/advanced_options_dialog.h
  include/pr2_interactive_manipulation/interactive_manipulation_frontend.h
  include/pr2_interactive_manipulation/camera_focus_frontend.h
  include/pr2_interactive_manipulation/point_head_camera_display.h
  include/pr2_interactive_manipulation/publish_click_camera_display.h
  include/pr2_interactive_manipulation/awesome_camera_display.h
  include/pr2_interactive_manipulation/invisible_robot_display.h
  include/pr2_interactive_manipulation/point_tools.h
)

# rviz plugin
add_library(interactive_manipulation_plugin 
  src/awesome_camera_display.cpp
  src/point_head_camera_display.cpp
  src/point_head_view_controller.cpp
  src/point_tools.cpp
  src/publish_click_camera_display.cpp
  src/publish_click_view_controller.cpp
  src/camera_focus_frontend.cpp
  src/interactive_manipulation_frontend.cpp
  src/interactive_manipulation_frontend_display.cpp
  src/interactive_manipulation_frontend.cpp
  src/advanced_options_dialog.cpp
  src/invisible_robot_display.cpp
  ${UIC_FILES}
  ${MOC_FILES}
)
target_link_libraries(interactive_manipulation_plugin 
  default_plugin # from rviz
  ${QT_LIBRARIES}
  ${OGRE_LIBRARIES}
)
## Generate added messages and services with any dependencies listed here
#generate_messages(
#    #TODO DEPENDENCIES geometry_msgs std_msgs
#)


# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS rviz object_manipulator tf pr2_interactive_object_detection_frontend object_recognition_gui pr2_controllers_msgs pr2_object_manipulation_msgs object_manipulation_msgs sensor_msgs std_msgs std_srvs pr2_msgs pr2_object_manipulation_msgs 
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  PATTERN ".svn" EXCLUDE
)

install(DIRECTORY config/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/config
  PATTERN ".svn" EXCLUDE
)

install(DIRECTORY images/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/images
  PATTERN ".svn" EXCLUDE
)

install(DIRECTORY icons/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/icons
  PATTERN ".svn" EXCLUDE
)

install(DIRECTORY image_src/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/image_src
  PATTERN ".svn" EXCLUDE
)

