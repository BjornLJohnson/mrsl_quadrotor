cmake_minimum_required(VERSION 2.8.3)
project(quadrotor_msgs)

# set default build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif()

find_package(catkin REQUIRED COMPONENTS message_generation geometry_msgs nav_msgs cmake_modules)
find_package(Eigen3 REQUIRED)

include_directories(include ${catkin_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR})

add_message_files(DIRECTORY msg
  FILES
  AuxCommand.msg
  Corrections.msg
  FlatOutputs.msg
  LineTrackerGoal.msg
  OutputData.msg
  PositionCommand.msg
  PWMCommand.msg
  Serial.msg
  SO3Command.msg
  StatusData.msg
  TRPYCommand.msg
  TrackerStatus.msg)

generate_messages(
  DEPENDENCIES geometry_msgs std_msgs)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES encode_msgs decode_msgs
  CATKIN_DEPENDS message_runtime geometry_msgs nav_msgs
  DEPENDS Eigen)

add_library(encode_msgs src/encode_msgs.cpp)
add_dependencies(encode_msgs ${${PROJECT_NAME}_EXPORTED_TARGETS})
target_link_libraries(encode_msgs ${catkin_LIBRARIES})

add_library(decode_msgs src/decode_msgs.cpp)
add_dependencies(decode_msgs ${${PROJECT_NAME}_EXPORTED_TARGETS})
target_link_libraries(decode_msgs ${catkin_LIBRARIES})

install(TARGETS encode_msgs decode_msgs
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/ DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
