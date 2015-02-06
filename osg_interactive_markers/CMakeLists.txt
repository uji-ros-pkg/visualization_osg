cmake_minimum_required(VERSION 2.8.3)
project(osg_interactive_markers)
find_package(catkin REQUIRED COMPONENTS
  roscpp
  tf
  osg_utils
  osg_markers
  interactive_markers
)

find_package(osg_utils REQUIRED)
find_package(osg_markers REQUIRED)

catkin_package(
INCLUDE_DIRS include
LIBRARIES ${PROJECT_NAME}
CATKIN_DEPENDS
  roscpp
  tf
  osg_utils
  osg_markers
  interactive_markers
DEPENDS
  libopenscenegraph
)

FIND_PACKAGE ( OpenSceneGraph COMPONENTS osgSim osgUtil osgDB osgFX osgGA osgTerrain osgViewer osgText osgWidget osgManipulator osg)
message("OPENSCENEGRAPH_LIBRARIES= ${OPENSCENEGRAPH_LIBRARIES}")

#link_directories(${VISP_LIBRARY_DIRS} ${Boost_LIBRARY_DIRS})
include_directories(include ${catkin_INCLUDE_DIRS})
add_library(${PROJECT_NAME}
  src/interactive_marker_client.cpp
  src/interactive_marker_display.cpp
  src/interactive_marker.cpp
  src/interactive_marker_control.cpp
  src/draggers.cpp
)

add_executable(osg_interactive_markers_demo examples/osg_interactive_markers_demo.cpp)
target_link_libraries(osg_interactive_markers_demo ${PROJECT_NAME} ${catkin_LIBRARIES} ${OPENSCENEGRAPH_LIBRARIES} -lGLU )


add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})
install(
TARGETS
${PROJECT_NAME}
ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
install(DIRECTORY include/${PROJECT_NAME}
DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
