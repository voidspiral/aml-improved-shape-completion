cmake_minimum_required(VERSION 3.2)
project(libvoxelizecloud)

set(CMAKE_CXX_FLAGS "--std=gnu++11 ${CMAKE_CXX_FLAGS} -O3")
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

find_package(HDF5 COMPONENTS C CXX HL REQUIRED)
find_package(Boost COMPONENTS system filesystem REQUIRED)
find_package(JsonCPP REQUIRED)
find_package(Eigen3 REQUIRED)

message("Eigen3: ${EIGEN3_INCLUDE_DIR}")

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

include_directories(
  ${JsonCPP_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${HDF5_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
)
add_executable(voxelize_clouds voxelize_clouds.cpp box_ray/box.cc)
target_link_libraries(voxelize_clouds
  ${JsonCPP_LIBRARIES}
  ${Boost_LIBRARIES}
  ${HDF5_CXX_LIBRARIES}
)