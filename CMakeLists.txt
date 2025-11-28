cmake_minimum_required(VERSION 3.14)
project(PQC_HBS_Benchmark)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(liboqs REQUIRED)

if(NOT liboqs_FOUND)
    find_package(PkgConfig REQUIRED)
    pkg_check_modules(liboqs REQUIRED liboqs)

    add_library(liboqs::liboqs INTERFACE IMPORTED)
    target_include_directories(liboqs::liboqs INTERFACE ${liboqs_INCLUDE_DIRS})
    target_link_libraries(liboqs::liboqs INTERFACE ${liboqs_INCLUDE_DIRS})
endif()

add_executable(benchmark src/benchmark.cpp)
target_link_libraries(benchmark PRIVATE liboqs::liboqs)

if(MSVC)
    set_target_properties(benchmark PROPERTIES LINK_FLAGS "/STACK:10000000")
    set_property(TARGET benchmark PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreadedDLL")
endif()

add_executable(tester src/tester.cpp)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)