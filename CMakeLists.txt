cmake_minimum_required(VERSION 3.8)

project(httpserver)

# Modify include paths
include_directories(${PROJECT_SOURCE_DIR}/include)

# Use cppcheck to start static program analysis.
include(cmake/cppcheck.cmake)

# Set the Compilation options
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Werror -Wextra")

# Set the compilation options of the test program
set(CMAKE_DEBUG_C_FLAGS "-fsanitize=address -O1 -fno-omit-frame-pointer")
if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${CMAKE_DEBUG_C_FLAGS}")
endif()

# Set the compilation options for code coverage
option(CMAKE_C_COVERAGE "Enable Code Coverage" ON)
if (CMAKE_C_COVERAGE)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -coverage")
endif()

# Compile the test program
add_subdirectory(src/)
add_subdirectory(tests/)

# Generate targets for ctest and code coverage
include(cmake/ctest.cmake)
if (CMAKE_C_COVERAGE)
  include(cmake/coverage.cmake)
endif()