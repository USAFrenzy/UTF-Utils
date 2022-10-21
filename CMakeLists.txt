cmake_minimum_required(VERSION 3.14)

project(UTF-Utils)

option(
    BUILD_COMPILED_LIBRARY
    "Build The Sandbox Project Alongside The Library"
    OFF
)

if(BUILD_COMPILED_LIBRARY)
add_subdirectory(src)
endif()