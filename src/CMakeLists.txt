set(PROJECT_NAME UTF-Utils_Compiled)

if(BUILD_COMPILED_LIBRARY)

add_compile_definitions(BUILD_COMPILED_LIB)

set(UTF_UTILS_SOURCE_FILES
  "compiled.cpp"
)

set(UTF_UTILS_HEADER_FILES
  "${CMAKE_SOURCE_DIR}/utf-utils.h"
)

source_group(
    TREE "${CMAKE_SOURCE_DIR}"
    PREFIX "Header Files"
    FILES ${UTF_UTILS_HEADER_FILES}
)

source_group(
    TREE "${CMAKE_CURRENT_SOURCE_DIR}"
    PREFIX "Source Files"
    FILES ${UTF_UTILS_SOURCE_FILES}
)

set(UTF_UTILS_ALL_FILES ${UTF_UTILS_SOURCE_FILES} ${UTF_UTILS_HEADER_FILES})

add_library(${PROJECT_NAME} ${UTF_UTILS_ALL_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_SOURCE_DIR})

set(STANDARD 20)

set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES CXX_STANDARD ${STANDARD}
               CMAKE_ARCHIVE_OUTPUT_DIRECTORY_<CONFIG> ${CMAKE_SOURCE_DIR}/lib
               CMAKE_LIBRARY_OUTPUT_DIRECTORY_<CONFIG> ${CMAKE_SOURCE_DIR}/lib
               CMAKE_RUNTIME_OUTPUT_DIRECTORY_<CONFIG> ${CMAKE_SOURCE_DIR}/bin
               OUTPUT_NAME "utfutils"
               DEBUG_POSTFIX "_d"
)

endif()