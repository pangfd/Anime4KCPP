cmake_minimum_required(VERSION 3.14)
project(Anime4KCPP)

if(APPLE)
    # using brew's llvm (see https://stackoverflow.com/a/54715120/1410221)
    include_directories("/usr/local/include" "/usr/local/opt/llvm/include")
    link_directories("/usr/local/lib" "/usr/local/opt/llvm/lib")
endif()

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(WIN32)
    set(CMAKE_CXX_FLAGS "-O3 -fopenmp")
else()
    set(CMAKE_CXX_FLAGS "-O3 -lpthread -fopenmp")
endif()

if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    set(CMAKE_CXX_FLAGS "/EHsc")
endif()


set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
set(TOP_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

option(Build_GUI "Build GUI or not" ON)
option(Build_CLI "Build CLI or not" ON)
option(Build_VapourSynth_plugin "Build Anime4KCPP for VapourSynth plugin or not" OFF)
option(Build_AviSynthPlus_plugin "Build Anime4KCPP for AviSynthPlus plugin or not" OFF)
option(Build_C_wrapper "Build C wrapper of Anime4KCPP or not" OFF)
option(Build_C_wrapper_with_core "Build C wrapper and core in one file" OFF)
option(Built_in_kernel "Built-in kernel or not" ON)

set(VapourSynth_SDK_PATH "VapourSynth SDK PATH" CACHE PATH "Where to look for VapourSynth SDK")
set(AviSynthPlus_SDK_PATH "AviSynthPlus SDK PATH" CACHE PATH "Where to look for AviSynthPlus SDK")

macro(SUBDIRLIST result curdir)
    file(GLOB children RELATIVE ${curdir} ${curdir}/*)
    set(dirlist "")
    foreach(child ${children})
        if(IS_DIRECTORY ${curdir}/${child} AND EXISTS ${curdir}/${child}/CMakeLists.txt)
            list(APPEND dirlist ${child})
        endif()
    endforeach()
    set(${result} ${dirlist})
endmacro()

SUBDIRLIST(SUBDIRS ${CMAKE_CURRENT_SOURCE_DIR})

foreach(SUBDIR ${SUBDIRS})
    add_subdirectory(${SUBDIR})
endforeach()
