cmake_minimum_required(VERSION 3.16)

if("${CMAKE_CURRENT_SOURCE_DIR}" STREQUAL "${CMAKE_CURRENT_BINARY_DIR}")
    message(FATAL_ERROR "Only out-of-source builds are supported. \
                         Please use ${CMAKE_SOURCE_DIR}/cmake/InSourceCleanup.cmake \
                         in ${CMAKE_SOURCE_DIR} before attempting to generate project files again.")
endif()

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

if(DEFINED ENV{OpenCV_DIR} AND NOT DEFINED OpenCV_DIR)
    set(OpenCV_DIR $ENV{OpenCV_DIR})
endif()
if(NOT DEFINED OpenCV_DIR)
    message(FATAL_ERROR "OpenCV directory not specified; either define \"OpenCV_DIR\" as an environment \
                         variable or pass it straight to CMake.")
endif()

project(image-segmentation)

find_package(OpenCV REQUIRED)
find_package(OpenMP REQUIRED)

add_subdirectory("${CMAKE_SOURCE_DIR}/src" App)

set_target_properties(App PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
    COMPILE_DEFINITIONS "$<$<NOT:$<STREQUAL:CXX_COMPILER_ID,MSVC>>:-march=native>"
)