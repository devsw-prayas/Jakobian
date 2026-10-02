cmake_minimum_required(VERSION 3.20)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

project(Auto-Diff-Engine)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)
    message(WARNING
        "In-source build detected (configuring directly in the repo root). "
        "This scatters generated project files across every module directory "
        "and risks accidentally committing build artifacts. Prefer an "
        "out-of-source build: cmake -S . -B build")
endif()

set(PROJECT_BUILD_DIR "bin")
set(PROJECT_PDB_DIR "pdb")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_BUILD_DIR})
set(CMAKE_PDB_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_PDB_DIR})

if(MSVC)
    add_compile_options(/MP /Gy /Zc:inline)
else()
    add_compile_options(-ffunction-sections -fdata-sections)
    add_link_options(LINKER:--gc-sections)
endif()

add_subdirectory(Jakobian)
add_subdirectory(Tests)
