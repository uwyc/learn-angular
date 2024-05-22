# -*- mode:cmake -*-
project(Antlr4Demo LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(Antlr4Demo-GENERATED_SRC
    ${CMAKE_CURRENT_BINARY_DIR}/generated/TLexer.cpp
    ${CMAKE_CURRENT_BINARY_DIR}/generated/TParser.cpp
    ${CMAKE_CURRENT_BINARY_DIR}/generated/TParserBaseListener.cpp
    ${CMAKE_CURRENT_BINARY_DIR}/generated/TParserBaseVisitor.cpp
    ${CMAKE_CURRENT_BINARY_DIR}/generated/TParserListener.cpp
    ${CMAKE_CURRENT_BINARY_DIR}/generated/TParserVisitor.cpp
)

foreach(src_file ${Antlr4Demo-GENERATED_SRC})
    set_source_files_properties(
        ${src_file}
        PROPERTIES
        GENERATED TRUE
    )
endforeach()

add_custom_target(GenerateParser DEPENDS ${Antlr4Demo-GENERATED_SRC})
add_custom_command(OUTPUT ${Antlr4Demo-GENERATED_SRC}
    COMMAND
    ${CMAKE_COMMAND} -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/generated/
    COMMAND
    "${Java_JAVA_EXECUTABLE}" -jar ${ANTLR_JAR_LOCATION} -Werror -Dlanguage=Cpp
    -listener -visitor
    -o ${CMAKE_CURRENT_BINARY_DIR}/generated/
    -package antlrcpptest
    ${CMAKE_CURRENT_SOURCE_DIR}/TLexer.g4
    ${CMAKE_CURRENT_SOURCE_DIR}/TParser.g4
    WORKING_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}"
    DEPENDS
    ${CMAKE_CURRENT_SOURCE_DIR}/TLexer.g4
    ${CMAKE_CURRENT_SOURCE_DIR}/TParser.g4
)

if(NOT CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    set(flags_1 "-Wno-overloaded-virtual")
else()
    set(flags_1 "-MP /wd4251")
endif()

foreach(src_file ${Antlr4Demo-GENERATED_SRC})
    set_source_files_properties(
        ${src_file}
        PROPERTIES
        COMPILE_FLAGS "${COMPILE_FLAGS} ${flags_1}"
    )
endforeach(src_file ${Antlr4Demo_SRC})

add_executable(Antlr4Demo
    main.cpp
    ${Antlr4Demo-GENERATED_SRC}
)
target_include_directories(Antlr4Demo
    PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}/generated/
)

add_dependencies(Antlr4Demo GenerateParser)

target_link_libraries(Antlr4Demo antlr4_static)
