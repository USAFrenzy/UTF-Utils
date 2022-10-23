cmake_minimum_required(VERSION 3.14)

project(UTF-Utils)

option(
    BUILD_COMPILED_LIBRARY
    "Build The Sandbox Project Alongside The Library"
    OFF
)

set(UTF_UTILS_SOURCE_FILES
  "${CMAKE_SOURCE_DIR}/src/compiled.cpp"
)

set(UTF_UTILS_HEADER_FILES
  "${CMAKE_SOURCE_DIR}/include/UTF-Utils/utf-utils.h"
)

set(UTF_UTILS_ALL_FILES ${UTF_UTILS_SOURCE_FILES} ${UTF_UTILS_HEADER_FILES})

source_group(
    TREE "${CMAKE_SOURCE_DIR}/include/UTF-Utils"
    PREFIX "Header Files"
    FILES ${UTF_UTILS_HEADER_FILES}
)
source_group(
    TREE "${CMAKE_SOURCE_DIR}/src"
    PREFIX "Source Files"
    FILES ${UTF_UTILS_SOURCE_FILES}
)

if(BUILD_COMPILED_LIBRARY)
add_subdirectory(src)
endif()