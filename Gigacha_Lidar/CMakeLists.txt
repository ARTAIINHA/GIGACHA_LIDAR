cmake_minimum_required(VERSION 3.0.2)
project(pcl_cpp_tutorial)
set(CMAKE_CXX_STANDARD 17)

# 디버깅용 코드
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  pcl_conversions
  pcl_ros
  roscpp
  rospy
  sensor_msgs
  std_msgs
  geometry_msgs
  message_filters
)

find_package(PCL REQUIRED)  # PCL 추가

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES pcl_cpp_tutorial
  CATKIN_DEPENDS pcl_conversions pcl_ros roscpp sensor_msgs std_msgs message_filters geometry_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
    ${catkin_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS} 
    ${PROJECT_SOURCE_DIR}/include 
)

link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# ---------- LINKING ------------

# 만도 대회용 링킹
set(MANDO_SOURCES
    ${PROJECT_SOURCE_DIR}/src/main/practics_dynamic_mando.cpp
    ${PROJECT_SOURCE_DIR}/src/preprocessor.cpp
    ${PROJECT_SOURCE_DIR}/src/pointcloud_generator.cpp
    ${PROJECT_SOURCE_DIR}/src/utility.cpp
    ${PROJECT_SOURCE_DIR}/src/clustering.cpp
    ${PROJECT_SOURCE_DIR}/src/Hungarian.cpp
)
add_executable(${PROJECT_NAME}_mando ${MANDO_SOURCES})
target_link_libraries(${PROJECT_NAME}_mando ${catkin_LIBRARIES} ${PCL_LIBRARIES} )

add_executable(practics_dynamic_mando
  src/main/practics_dynamic_mando.cpp 
  src/preprocessor.cpp 
  src/pointcloud_generator.cpp
  src/utility.cpp
  src/clustering.cpp
  src/Hungarian.cpp
)
target_link_libraries(practics_dynamic_mando ${catkin_LIBRARIES} ${PCL_LIBRARIES}) 



# 임의의 GPS 생성
include_directories(include ${catkin_INCLUDE_DIRS})
add_executable(GPS_maker src/main/GPS_maker.cpp)
target_link_libraries(GPS_maker ${catkin_LIBRARIES})
