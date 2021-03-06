cmake_minimum_required(VERSION 3.0.2)
project(gesture_recognition_nuitrack)

add_compile_options(-std=c++14)

find_package(OpenCV REQUIRED)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  roslib

  std_srvs
  std_msgs
  sensor_msgs
  geometry_msgs
  nuitrack_msgs
  message_generation
  
  OpenCV
  cv_bridge
)

add_message_files(
  FILES
  gesture_recognized_array.msg
  gesture_recognized.msg
  pointat_direction.msg
)

add_service_files(
   FILES
   String.srv
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
  ${PROJECT_NAME}
)


catkin_package(
  INCLUDE_DIRS
    include
  LIBRARIES 
    #gesture_recognition_nuitrack
  CATKIN_DEPENDS
    roscpp
    rospy
    std_srvs
    std_msgs
    sensor_msgs
    geometry_msgs
    nuitrack_msgs
    message_runtime
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)

#################################################################################################
#                                           Libraries                                           #
#################################################################################################

# add_library(name_library 
# 	src/name_folder/source_file.cpp)
# target_link_libraries(source_file ${catkin_LIBRARIES})
# add_dependencies(name_library ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})


#################################################################################################
#                                          Executables                                          #
#################################################################################################

# Data Acquisition Node
add_executable(data_acquisition_Node 
	src/data_acquisition/data_acquisition_Node.cpp 
	src/data_acquisition/data_acquisition.cpp)
target_link_libraries(data_acquisition_Node ${catkin_LIBRARIES})
add_dependencies(data_acquisition_Node ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})

# Show Skeleton Node
add_executable(show_skeleton_Node 
	src/show_skeleton/show_skeleton_Node.cpp 
	src/show_skeleton/show_skeleton.cpp)
target_link_libraries(show_skeleton_Node ${catkin_LIBRARIES})
add_dependencies(show_skeleton_Node ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})

#################################################################################################
#                                      Install Python Nodes                                     #
#################################################################################################

install(
  
  PROGRAMS 
    src/dataset_creation.py
    src/gesture_recognition.py

  DESTINATION 
    ${CATKIN_PACKAGE_PYTHON_DESTINATION}

)
