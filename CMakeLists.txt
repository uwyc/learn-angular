cmake_minimum_required(VERSION 3.23.0)

project(LearnAntlr4)

if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/.git" AND EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/.gitmodules")
    find_package(Git REQUIRED)

    execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
            WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
            RESULT_VARIABLE GIT_SUBMODULE_RESULT)
endif()

add_subdirectory(3rdparty)
add_subdirectory(Antlr4Demo)
