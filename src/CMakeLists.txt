set(PROJECT_NAME UTF-Utils_Compiled)

if(BUILD_COMPILED_LIBRARY)

add_compile_definitions(BUILD_COMPILED_LIB)

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