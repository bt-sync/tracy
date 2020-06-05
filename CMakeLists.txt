set(PROJECT_ROOT ../..)
include(${PROJECT_ROOT}/cmake/cmake_version.cmake)
cmake_minimum_required(VERSION ${CMAKE_MIN_VERSION} FATAL_ERROR)

get_filename_component(PROJECT_NAME ${CMAKE_CURRENT_SOURCE_DIR} NAME)

project(${PROJECT_NAME} LANGUAGES CXX)
set(PLATFORM_TAG "")

set(CMAKE_CXX_STANDARD 14)

include(${PROJECT_ROOT}/cmake/utils.cmake)

get_platform_tag(PLATFORM_TAG)
include(${PROJECT_ROOT}/cmake/${PLATFORM_TAG}/platform.cmake)

platform_setup()
platform_setup_cxx_flags()
platform_setup_linker_flags()

if (${CMAKE_ENABLE_TRACY})

	file(GLOB SRC_FILES TracyClient.cpp)

	add_definitions(-DTRACY_ENABLE
	)

	add_library(${PROJECT_NAME} STATIC
	    ${SRC_FILES}
	)

endif()


target_include_directories(${PROJECT_NAME} 
	INTERFACE
	"${CMAKE_CURRENT_LIST_DIR}")
