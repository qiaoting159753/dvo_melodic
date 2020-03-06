cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
rosbuild_init()

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


find_package(Sophus)
find_package(Eigen3 REQUIRED)
find_package(OpenCV)
find_package(PCL 1.9 REQUIRED)
find_package(VTK REQUIRED)

include_directories(${Eigen3_INCLUDE_DIRS})
include_directories(${PCL_INCLUDE_DIRS})
include_directories(${VTK_INCLUDE_DIRS})
include_directories("${PROJECT_SOURCE_DIR}/include")

# SSE flags
rosbuild_check_for_sse()

set(CMAKE_CXX_FLAGS_RELWITHDEBINFO
   "${SSE_FLAGS} -O3 -march=native"
)

add_definitions( ${PCL_DEFINITIONS} )
link_directories( ${PCL_LIBRARY_DIRS} )

rosbuild_add_library(${PROJECT_NAME} 
    src/core/interpolation.cpp
    src/core/intrinsic_matrix.cpp
    src/core/least_squares.cpp
    src/core/math_sse.cpp
    src/core/rgbd_image.cpp
    src/core/rgbd_image_sse.cpp
    src/core/surface_pyramid.cpp
    src/core/weight_calculation.cpp
    src/util/histogram.cpp
    src/visualization/async_point_cloud_builder.cpp
    src/visualization/point_cloud_aggregator.cpp
    src/visualization/visualizer.cpp
    src/visualization/camera_trajectory_visualizer.cpp
    src/visualization/pcl_camera_trajetory_visualizer.cpp
    src/dense_tracking.cpp
    src/dense_tracking_config.cpp
)

target_link_libraries(${PROJECT_NAME} 
    tbb
    ${OpenCV_LIBS}
    boost_thread
    ${EIGEN3_LIBS}
    ${PCL_LIBRARIES}
)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${Sophus_LIBRARIES})
