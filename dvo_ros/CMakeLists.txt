cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
rosbuild_init()
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# Eigen3 library
find_package(Boost COMPONENTS system thread REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(PCL 1.9 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
include_directories(${Eigen3_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIRS})

add_definitions( ${PCL_DEFINITIONS} )
link_directories( ${PCL_LIBRARY_DIRS} )

# dynamic reconfigure
rosbuild_find_ros_package(dynamic_reconfigure)
include(${dynamic_reconfigure_PACKAGE_PATH}/cmake/cfgbuild.cmake)
gencfg()

rosbuild_add_library(${PROJECT_NAME} 
    src/camera_base.cpp
    src/camera_dense_tracking.cpp
    src/visualization/ros_camera_trajectory_visualizer.cpp
)

rosbuild_add_library(camera_tracker_nodelet
    src/camera_tracker_nodelet.cpp
)

target_link_libraries(camera_tracker_nodelet
	Eigen3::Eigen
	${OpenCV_LIBS}
	${Boost_LIBRARIES}
	${Boost_SYSTEM_LIBRARY}
    ${PROJECT_NAME}
    ${PCL_LIBRARIES}
)

rosbuild_add_executable(camera_tracker
     src/camera_tracker_node.cpp
)

target_link_libraries(${PROJECT_NAME} ${PCL_LIBRARIES} ${OpenCV_LIBS})

target_link_libraries(camera_tracker
	Eigen3::Eigen
	${OpenCV_LIBS}
	${Boost_LIBRARIES}
	${Boost_SYSTEM_LIBRARY}
    ${PROJECT_NAME}
    ${PCL_LIBRARIES}
)