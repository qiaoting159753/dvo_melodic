cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

rosbuild_init()

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

find_package(PCL 1.9 REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(VTK REQUIRED)
find_package(OpenCV)

include_directories(${Eigen3_INCLUDE_DIRS})
include_directories(${PCL_INCLUDE_DIRS})
include_directories(${VTK_INCLUDE_DIRS})

link_directories(${VTK_LIBRARY_DIRS})
add_definitions( ${PCL_DEFINITIONS} )
link_directories( ${PCL_LIBRARY_DIRS} )

rosbuild_add_executable(benchmark
     src/benchmark.cpp
)

target_link_libraries(benchmark
  ${VTK_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBS}
)

# create output directory - just convenience for our users
file(MAKE_DIRECTORY "${PROJECT_SOURCE_DIR}/output")
file(MAKE_DIRECTORY "${PROJECT_SOURCE_DIR}/output/video")
