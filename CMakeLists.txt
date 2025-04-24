cmake_minimum_required(VERSION 3.13)

project(pointcloud)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

file(GLOB LIB_SOURCES "lib/*.c" "lib/*.cpp")

set(PGSQL_SOURCES
	"pgsql/pc_editor.c"
	"pgsql/pc_pgsql.c"
)

set(SOURCES ${LIB_SOURCES} ${PGSQL_SOURCES})

add_library(pointcloud SHARED ${SOURCES})

if(WIN32)
        set_target_properties(pointcloud PROPERTIES LINK_FLAGS "/DEF:${CMAKE_CURRENT_SOURCE_DIR}/exports.def" OUTPUT_NAME "libpointcloud")
endif()

if(NOT DEFINED LAZPERF_INCLUDE_DIR)
        message(FATAL_ERROR "LAZPERF_INCLUDE_DIR is not defined! Please provide the path to LazPerf include directory.")
endif()

target_include_directories(pointcloud PUBLIC 
	${CMAKE_SOURCE_DIR}/include
	${CMAKE_SOURCE_DIR}/lib
	${LAZPERF_INCLUDE_DIR}
)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

install(TARGETS pointcloud DESTINATION lib)
