cmake_minimum_required(VERSION 3.20)
project(Jakobian LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

file(GLOB_RECURSE Jakobian_HEADERS  CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/Public/*.h")
file(GLOB_RECURSE Jakobian_SOURCE   CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/Private/*.cpp")

add_library(Jakobian SHARED
    ${Jakobian_HEADERS}
    ${Jakobian_SOURCE}
)
target_compile_definitions(Jakobian PRIVATE JAKOBIAN_SHARED)
target_include_directories(Jakobian PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/Public
)

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES
    ${Jakobian_HEADERS}
    ${Jakobian_SOURCE}
)

target_precompile_headers(Jakobian PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/Public/Jakobian.h)

target_compile_options(Jakobian PRIVATE
    $<$<AND:$<COMPILE_LANGUAGE:CXX>,$<CXX_COMPILER_ID:MSVC>>:/arch:AVX2 /W4 /permissive->
    $<$<AND:$<COMPILE_LANGUAGE:CXX>,$<NOT:$<CXX_COMPILER_ID:MSVC>>>:-mavx2 -Wall -Wextra>
)
