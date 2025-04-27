cmake_minimum_required (VERSION 3.7.0) 
project(ZipLib)

# Option to build tests/examples (off by default for FetchContent usage)
option(ZIPLIB_BUILD_TESTS "Build ZipLib tests and examples" OFF)

# Only add subdirectories if not already present (for FetchContent compatibility)
if(NOT TARGET bzip2)
    add_subdirectory(extlibs/bzip2)
endif()
if(NOT TARGET lzma)
    add_subdirectory(extlibs/lzma)
endif()
if(NOT TARGET zlib)
    add_subdirectory(extlibs/zlib)
endif()

file(GLOB headers *.h detail/*.h)
file(GLOB sources *.cpp detail/*.cpp)

add_library(ZipLib STATIC ${headers} ${sources})
add_library(ZipLib::ZipLib ALIAS ZipLib)

# Public include directories for consumers
# (Assumes headers are in the root and detail/)
target_include_directories(ZipLib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(ZipLib PUBLIC bzip2 lzma zlib)

set_target_properties(ZipLib PROPERTIES FOLDER "00.Libs")