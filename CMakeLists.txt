cmake_minimum_required(VERSION 2.8.3)
project(dmp)

find_package(catkin REQUIRED
  COMPONENTS roscpp rospy std_msgs message_generation
)

find_package(cmake_modules REQUIRED)

add_message_files(#DIRECTORY msg
  FILES DMPData.msg DMPPoint.msg DMPTraj.msg
)

add_service_files(#DIRECTORY srv
  FILES GetDMPPlan.srv LearnDMPFromDemo.srv SetActiveDMP.srv
)

generate_messages(DEPENDENCIES std_msgs) 

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES dmp
  CATKIN_DEPENDS message_runtime std_msgs
  DEPENDS Eigen
)

find_package(Eigen REQUIRED)
include_directories(include ${EIGEN_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})
add_definitions(${EIGEN_DEFINITIONS})

add_library(dmp src/dmp.cpp src/fourier_approx.cpp src/radial_approx.cpp)
add_executable(dmp_server nodes/dmp_server.cpp)

add_dependencies(dmp ${PROJECT_NAME}_gencpp)
add_dependencies(dmp_server ${PROJECT_NAME}_gencpp)

target_link_libraries(dmp_server dmp ${catkin_LIBRARIES})

