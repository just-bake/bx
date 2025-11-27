cmake_minimum_required(VERSION 3.16)
project(bx VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(BX_DIR "${CMAKE_CURRENT_SOURCE_DIR}" CACHE PATH "Path to bx root directory")
set(BX_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include" CACHE PATH "Path to bx include directory")
set(BX_SCRIPTS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/scripts" CACHE PATH "Path to bx scripts directory")

# Options
option(BX_INSTALL "install bx" OFF)
option(BX_BUILD_TESTS "Build bx tests" OFF)

add_subdirectory(src)
# Build tests
if(BX_BUILD_TESTS)
	enable_testing()
	add_subdirectory(tests)
endif()
