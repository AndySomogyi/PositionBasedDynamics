cmake_minimum_required(VERSION 2.8)

project(PositionBasedDynamics)

set(PROJECT_PATH ${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR})

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/CMake" ${CMAKE_MODULE_PATH})
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

include(${PROJECT_PATH}/CMake/Common.cmake)


# set the eigen include dir, only uses eigen as headers
set(EIGEN3_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/extern/eigen")

message("EIGEN3_INCLUDE_DIR: ${EIGEN3_INCLUDE_DIR}")

add_subdirectory(extern)

add_subdirectory(Demos)

add_subdirectory(PositionBasedDynamics)

install(DIRECTORY ./Common
DESTINATION include
FILES_MATCHING PATTERN "*.h")

install(DIRECTORY ./data
DESTINATION include
FILES_MATCHING PATTERN "*.glsl")

