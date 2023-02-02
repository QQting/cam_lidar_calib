cmake_minimum_required(VERSION 2.8.3)
set (CMAKE_CXX_STANDARD 14)
project(cam_lidar_calib)

find_package(catkin REQUIRED COMPONENTS
  pcl_conversions
  pcl_ros
  roscpp
  sensor_msgs
  message_filters
  cv_bridge
  camera_calibration_parsers
  tf
  tf_conversions
  message_generation
)

find_package( PCL REQUIRED)
find_package( OpenCV REQUIRED)
find_package( Ceres REQUIRED)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${CERES_INCLUDE_DIRS}
)

add_message_files(
  FILES
  ImageDepth.msg
)
generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)
catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS message_runtime
  DEPENDS PCL
)

add_executable(${PROJECT_NAME}_node src/cam_lidar_calib_node.cpp)

add_executable(cam_lidar_proj_node src/cam_lidar_proj_node.cpp)

add_dependencies(cam_lidar_proj_node ${catkin_EXPORTED_TARGETS})

target_link_libraries(${PROJECT_NAME}_node
   ${catkin_LIBRARIES}
   ${OpenCV_LIBS}
   ${CERES_LIBRARIES}
)

target_link_libraries(cam_lidar_proj_node
        ${catkin_LIBRARIES}
        ${OpenCV_LIBS}
        )
