set(WITH_STATIC_CRT OFF)
set(DISABLE_WARNINGS ON)
set(ANTLR_BUILD_CPP_TESTS OFF)
set(ANTLR_BUILD_SHARED OFF)
add_subdirectory(antlr4/runtime/Cpp)

target_include_directories(antlr4_static INTERFACE
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp/runtime/src
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp/runtime/src/misc
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp/runtime/src/atn
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp/runtime/src/dfa
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp/runtime/src/tree
    ${CMAKE_CURRENT_LIST_DIR}/antlr4/runtime/Cpp/runtime/src/support
)

find_package(Java COMPONENTS Runtime REQUIRED)
find_program(ANTLR_JAR_LOCATION
    NAMES antlr.jar antlr4.jar antlr-4.jar antlr-4.13.1-complete.jar
    HINTS ${CMAKE_CURRENT_LIST_DIR} ${CMAKE_CURRENT_SOURCE_DIR} ${PROJECT_SOURCE_DIR})

if(NOT ANTLR_JAR_LOCATION)
    message(FATAL_ERROR "Missing antlr4.jar location. You can specify it's path using: -DANTLR_JAR_LOCATION=<path>")
else()
    get_filename_component(ANTLR_NAME ${ANTLR_JAR_LOCATION} NAME_WLE)

    if(NOT EXISTS "${ANTLR_JAR_LOCATION}")
        message(FATAL_ERROR "Unable to find ${ANTLR_NAME} in ${ANTLR_JAR_LOCATION}")
    else()
        message(STATUS "Found ${ANTLR_NAME}: ${ANTLR_JAR_LOCATION}")
    endif()
endif()