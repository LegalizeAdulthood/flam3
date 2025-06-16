cmake_minimum_required(VERSION 3.23...3.31)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
include(misc_target)
include(target_folder)
include(vs_startup_project)

set(FLAM3_PROJECT_NAME "flam3" CACHE "STRING" "Default project name for IDEs")
project("${FLAM3_PROJECT_NAME}" VERSION "3.1.1" LANGUAGES C)

find_package(ZLIB REQUIRED)
find_package(PNG REQUIRED)
find_package(LibXml2 REQUIRED)
find_package(jpeg REQUIRED)

add_library(flam3
    config.h
    filters.h
    flam3.h
    img.h
    interpolation.h
    isaac.h
    isaacs.h
    palettes.h
    parser.h
    private.h
    variations.h
    docstring.c
    filters.c
    flam3.c
    interpolation.c
    isaac.c
    jpeg.c
    palettes.c
    parser.c
    png.c
    variations.c
)
target_include_directories(flam3 PUBLIC .)
target_link_libraries(flam3 PUBLIC JPEG::JPEG PNG::PNG LibXml2::LibXml2 ZLIB::ZLIB Ws2_32)
target_folder(flam3 "Libraries")
add_executable(flam3-animate flam3-animate.c)
target_link_libraries(flam3-animate PRIVATE flam3)
target_folder(flam3-animate "Applications")
add_executable(flam3-convert flam3-convert.c)
target_link_libraries(flam3-convert PRIVATE flam3)
target_folder(flam3-convert "Applications")
add_executable(flam3-genome flam3-genome.c)
target_link_libraries(flam3-genome PRIVATE flam3)
target_folder(flam3-genome "Applications")
add_executable(flam3-render flam3-render.c)
target_link_libraries(flam3-render PRIVATE flam3)
target_folder(flam3-render "Applications")

vs_startup_project(flam3)

misc_target(FILES
    "CMakePresets.json"
    "README.txt")
misc_group_sources("CMake Scripts" FILES
    "cmake/misc_target.cmake"
    "cmake/target_folder.cmake"
    "cmake/vs_startup_project.cmake")
misc_group_sources("Editing" FILES
    ".clang-format"
    ".editorconfig")
misc_group_sources("Git" FILES
    ".gitignore"
    ".gitmodules")
misc_group_sources("GitHub" FILES
    ".github/workflows/cmake.yml")
misc_group_sources("VcPkg" FILES
    "vcpkg.json")
