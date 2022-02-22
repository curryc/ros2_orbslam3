cmake_minimum_required(VERSION 3.5)
project(ros2_orbslam3)

# you should set the PYTHONPATH to your own python site-packages path, below is mine
set(ENV{PYTHONPATH} "/opt/ros/galactic/lib/python3.8/site-packages/")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules)

# MESSAGE("cmake model path" ${CMAKE_MODEL_PATH})

# Default to C++11
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 11)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# set(rclcpp_DIR /home/curry/ros2_galactic/install/rclcpp/share/rclcpp/cmake)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(message_filters REQUIRED)
find_package(Pangolin REQUIRED)
find_package(ORB_SLAM3 REQUIRED)

add_executable(mono 
  src/monocular/mono.cpp
  src/monocular/monocular-slam-node.cpp
)
ament_target_dependencies(mono rclcpp sensor_msgs cv_bridge ORB_SLAM3
 Pangolin)

add_executable(rgbd 
  src/rgbd/rgbd.cpp
  src/rgbd/rgbd-slam-node.cpp
)
ament_target_dependencies(rgbd rclcpp sensor_msgs cv_bridge message_filters
ORB_SLAM3
 Pangolin)

add_executable(stereo 
  src/stereo/stereo.cpp
  src/stereo/stereo-slam-node.cpp
)

ament_target_dependencies(stereo rclcpp sensor_msgs cv_bridge message_filters 
ORB_SLAM3
 Pangolin)

install(TARGETS mono rgbd stereo
  DESTINATION lib/${PROJECT_NAME})

ament_package()

