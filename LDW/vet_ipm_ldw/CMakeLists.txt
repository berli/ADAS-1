cmake_minimum_required (VERSION 2.6)
project(nsfw)

# Use C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
message(STATUS "C++11 support has been enabled by default.")
# Use SSE
option(USE_SSE         "Set to ON to build use SSE"  ON)
option(USE_OPENMP      "Set to ON to build use openmp"  ON)
# The version number.
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")

if (USE_SSE)
    add_definitions(-DUSE_SSE)
    message(STATUS "Use SSE")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse4.1")
endif()

add_definitions(${Caffe_DEFINITIONS})

find_package(OpenCV)
if (NOT OpenCV_FOUND)
    message(WARNING "OpenCV not found. Test will not be built.")
else()
    include_directories(${OpenCV_INCLUDE_DIRS})
   # list(APPEND _required_libs ${OpenCV_LIBS})
   # 
   # add_executable(facedet_test src/test/facedetection_test.cpp)
   # target_link_libraries(facedet_test ${facedet_required_libs})
endif()

find_package( Caffe REQUIRED )
if (NOT Caffe_FOUND)
    message(WARNING "Caffe not found. ")
 else()
    include_directories(${Caffe_INCLUDE_DIRS})
endif()
# add the binary tree to the search path for include files
# so that we will find nsfwConfig.h
include_directories("${PROJECT_BINARY_DIR}")

# add the executable

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)
add_executable(nsfw ${DIR_SRCS} )
target_link_libraries (nsfw  ${OpenCV_LIBS} )

