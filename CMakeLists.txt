﻿cmake_minimum_required(VERSION 3.8)

project("Volumetric Clouds")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(XPLM_SDK_INCLUDE_DIRECTORY "" CACHE PATH "X-Plane SDK include directory")
set(XPLM_SDK_LIBRARY_FILE "" CACHE FILEPATH "X-Plane SDK library file (leave empty on Linux)")

find_package(OpenGL REQUIRED)

set(GLEW_INCLUDE_DIRECTORY "" CACHE PATH "GLEW include directory")
set(GLEW_LIBRARY_FILE "" CACHE FILEPATH "GLEW library file")

set(ZLIB_INCLUDE_DIRECTORY "" CACHE PATH "zlib include directory")
set(ZLIB_LIBRARY_FILE "" CACHE FILEPATH "zlib library file")

set(LIBPNG_INCLUDE_DIRECTORY "" CACHE PATH "libpng include directory")
set(LIBPNG_LIBRARY_FILE "" CACHE FILEPATH "libpng library file")

add_library(volumetric_clouds SHARED "include/dataref_helpers.hpp" "include/opengl_helpers.hpp" "src/dataref_helpers.cpp" "src/opengl_helpers.cpp" "src/volumetric_clouds.cpp")

target_include_directories(volumetric_clouds PRIVATE "include" ${XPLM_SDK_INCLUDE_DIRECTORY} ${GLEW_INCLUDE_DIRECTORY} ${ZLIB_INCLUDE_DIRECTORY} ${LIBPNG_INCLUDE_DIRECTORY})
target_link_libraries(volumetric_clouds PRIVATE ${XPLM_SDK_LIBRARY_FILE} OpenGL::GL ${GLEW_LIBRARY_FILE} ${ZLIB_LIBRARY_FILE} ${LIBPNG_LIBRARY_FILE})

target_compile_definitions(volumetric_clouds PRIVATE "XPLM200=1" "XPLM210=1" "XPLM300=1" "XPLM301=1" "XPLM302=1" "XPLM303=1" "XPLM_DEPRECATED=1")

set_target_properties(volumetric_clouds PROPERTIES PREFIX "")
set_target_properties(volumetric_clouds PROPERTIES SUFFIX ".xpl")

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
	target_compile_definitions(volumetric_clouds PRIVATE "NOMINMAX=1" "IBM=1" "GLEW_BUILD=1")

	set_target_properties(volumetric_clouds PROPERTIES OUTPUT_NAME "win")
	
	install(DIRECTORY "Volumetric Clouds" DESTINATION ${CMAKE_INSTALL_PREFIX})
	install(TARGETS volumetric_clouds RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}/Volumetric Clouds")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	target_compile_definitions(volumetric_clouds PRIVATE "LIN=1")

	set_target_properties(volumetric_clouds PROPERTIES OUTPUT_NAME "lin")

	install(DIRECTORY "Volumetric Clouds" DESTINATION ${CMAKE_INSTALL_PREFIX})
	install(TARGETS volumetric_clouds LIBRARY DESTINATION "${CMAKE_INSTALL_PREFIX}/Volumetric Clouds")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
	target_compile_definitions(volumetric_clouds PRIVATE "APL=1")

	set_target_properties(volumetric_clouds PROPERTIES OUTPUT_NAME "mac")

	install(DIRECTORY "Volumetric Clouds" DESTINATION ${CMAKE_INSTALL_PREFIX})
	install(TARGETS volumetric_clouds FRAMEWORK DESTINATION "${CMAKE_INSTALL_PREFIX}/Volumetric Clouds")
endif()