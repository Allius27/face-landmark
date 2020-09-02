cmake_minimum_required(VERSION 2.8.7)
project(face_landmark)

SET( CMAKE_CXX_FLAGS "-O3 -std=c++11" )

set(caffe_DIR "" CACHE STRING "caffe install dir")
set(dlib_DIR  "" CACHE STRING "dlib install dir")

include_directories(${caffe_DIR}/include)
include_directories(${dlib_DIR}/include)

link_directories(${caffe_DIR}/lib)
link_directories(${dlib_DIR}/lib)

option(CAFFE_CPU_ONLY "Enable cpu only build" OFF)

if(CAFFE_CPU_ONLY)
    add_definitions(-DCPU_ONLY)
endif(CAFFE_CPU_ONLY)
#unset(CAFFE_CPU_ONLY CACHE) # <---- this is the important!!


FIND_PACKAGE( OpenCV REQUIRED )

add_executable(face_landmark face_landmark.cpp)
target_link_libraries(face_landmark
		${OpenCV_LIBS}
		caffe
		dlib 
		glog
		gflags
		protobuf
		boost_system)

add_executable(rotation_image rotation_image.cpp)
target_link_libraries(rotation_image 
		caffe	
		${OpenCV_LIBS}
		glog
		gflags
		protobuf)

INSTALL (
	FILES 
	${CMAKE_CURRENT_BINARY_DIR}/face_landmark
	${CMAKE_CURRENT_BINARY_DIR}/rotation_image 
	DESTINATION bin
)

INSTALL (
	FILES 
	result/1.jpg
	result/2.jpg
	result/4.jpg
	result/7.jpg
	result/8.jpg
	DESTINATION img
)

INSTALL (
	DIRECTORY model  DESTINATION .
)