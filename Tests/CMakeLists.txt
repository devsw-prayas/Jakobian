cmake_minimum_required(VERSION 3.20)
project(Tests LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

file(GLOB_RECURSE Tests_HEADERS  CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/Public/*.h")
file(GLOB_RECURSE Tests_SOURCE   CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/Private/*.cpp")

add_executable(Tests
    ${Tests_HEADERS}
    ${Tests_SOURCE}
)
target_include_directories(Tests PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/Public
)

target_link_libraries(Tests PRIVATE Jakobian)

target_precompile_headers(Tests PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/Public/Tests.h)

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES
    ${Tests_HEADERS}
    ${Tests_SOURCE}
)

target_compile_options(Tests PRIVATE
    $<$<AND:$<COMPILE_LANGUAGE:CXX>,$<CXX_COMPILER_ID:MSVC>>:/arch:AVX2 /W4 /permissive->
    $<$<AND:$<COMPILE_LANGUAGE:CXX>,$<NOT:$<CXX_COMPILER_ID:MSVC>>>:-mavx2 -Wall -Wextra>
)