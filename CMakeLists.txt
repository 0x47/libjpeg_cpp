cmake_minimum_required(VERSION 3.0)

project(libjpeg_cpp)
set(tname libjpeg_cpp)

set(CMAKE_CXX_STANDARD 14) # -std=c++14
#set(CMAKE_CXX_STANDARD_REQUIRED ON) # do not fall back
#set(CMAKE_CXX_EXTENSIONS OFF) # no -std=gnu*

#set(CMAKE_VERBOSE_MAKEFILE true)
#set(CMAKE_POSITION_INDEPENDENT_CODE ON) # -fPIC

list(APPEND warning_flags
	"-Wall"
	"-Wextra"
	"-Wpedantic"
	#"-Werror"
)

set(header_files
	${CMAKE_CURRENT_SOURCE_DIR}/jpeg.h
)

set(source_files
	${CMAKE_CURRENT_SOURCE_DIR}/jpeg.cpp
)

set(source_files_test
	${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
)

find_library(LIBJPEG_CPP_LIBJPEG_LIB jpeg)
if(LIBJPEG_CPP_LIBJPEG_LIB-NOTFOUND)
	message(FATAL_ERROR "LibJPEG not found")
endif()
list(APPEND external_libraries ${LIBJPEG_CPP_LIBJPEG_LIB})

add_library(${tname} ${source_files} ${header_files})
target_link_libraries(${tname} PUBLIC ${external_libraries})
target_include_directories(${tname} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_options(${tname} PUBLIC ${warning_flags})

add_executable(${tname}_test EXCLUDE_FROM_ALL ${source_files_test})
target_link_libraries(${tname}_test PUBLIC ${tname})
target_compile_options(${tname}_test PUBLIC ${warning_flags})
