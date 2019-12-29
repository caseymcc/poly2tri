cmake_minimum_required (VERSION 3.0)

option(POLY3TRI_TESTBED "Builds test application" OFF)

set(HUNTER_STATUS_DEBUG ON)
set(
    HUNTER_CACHE_SERVERS
    "https://github.com/huntercache/voxigen"
    CACHE
    STRING
    "Default cache server"
)

include(${CMAKE_CURRENT_LIST_DIR}/cmake/Hunter/HunterGate.cmake)

HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.23.204.tar.gz"
    SHA1 "32cfed254da901f6f184027d530d8da47e035b85"
#    LOCAL
)

project(poly2tri)

set(poly2tri_headers
    poly2tri/poly2tri.h
    poly2tri/common/shapes.h
    poly2tri/common/utils.h
    poly2tri/sweep/advancing_front.h
    poly2tri/sweep/cdt.h
    poly2tri/sweep/sweep.h
    poly2tri/sweep/sweep_context.h
)

set(poly2tri_sources
    poly2tri/common/shapes.cc
    poly2tri/sweep/advancing_front.cc
    poly2tri/sweep/cdt.cc
    poly2tri/sweep/sweep.cc
    poly2tri/sweep/sweep_context.cc
)

add_library(poly2tri 
    ${poly2tri_headers}
    ${poly2tri_sources}
)

if(POLY3TRI_TESTBED)

    find_package(OpenGL REQUIRED)
    hunter_add_package(glfw)
    find_package(glfw3 CONFIG REQUIRED)

    set(poly2tri_testbed_libraries
        poly2tri
        OpenGL::GL
        glfw
    )

    add_executable(poly2tri_testbed testbed/main.cc)
    target_link_libraries(poly2tri_testbed ${poly2tri_testbed_libraries})

endif()

