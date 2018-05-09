cmake_minimum_required(VERSION 2.8.3)
project(dmp)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(cmake_modules)

find_package(catkin REQUIRED COMPONENTS
roscpp
rospy
message_generation
)

add_message_files(
   FILES
   DMPData.msg
   DMPPoint.msg
   DMPTraj.msg
 )

add_service_files(
   FILES
   GetDMPPlan.srv
   LearnDMPFromDemo.srv
   SetActiveDMP.srv
 )

generate_messages()

find_package(Eigen REQUIRED)

catkin_package(
INCLUDE_DIRS include
LIBRARIES dmp
CATKIN_DEPENDS std_msgs message_runtime
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

add_library(dmp src/dmp.cpp src/fourier_approx.cpp src/radial_approx.cpp src/linear_approx)
add_executable(dmp_server nodes/dmp_server.cpp)
target_link_libraries(dmp_server dmp ${catkin_LIBRARIES})
add_dependencies(dmp dmp_generate_messages_cpp)
add_dependencies(dmp_server dmp_generate_messages_cpp)
