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

# Install rules
if(BX_INSTALL)
	# Install include directory
	install(DIRECTORY "${BX_INCLUDE_DIR}" DESTINATION include)

	# Install scripts directory
	install(DIRECTORY "${BX_SCRIPTS_DIR}" DESTINATION scripts)

	# Install binaries or libraries from src
	install(TARGETS bx EXPORT bxTargets
		RUNTIME DESTINATION bin
		LIBRARY DESTINATION lib
		ARCHIVE DESTINATION lib
	)

	# Install CMake configuration files
	include(CMakePackageConfigHelpers)
	write_basic_package_version_file(
		"${CMAKE_CURRENT_BINARY_DIR}/bxConfigVersion.cmake"
		VERSION ${PROJECT_VERSION}
		COMPATIBILITY AnyNewerVersion
	)
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/cmake/bxConfig.cmake.in"
		"${CMAKE_CURRENT_BINARY_DIR}/bxConfig.cmake"
		@ONLY
	)
	install(FILES
		"${CMAKE_CURRENT_BINARY_DIR}/bxConfig.cmake"
		"${CMAKE_CURRENT_BINARY_DIR}/bxConfigVersion.cmake"
		DESTINATION lib/cmake/bx
	)

	# Export the targets for find_package
	install(EXPORT bxTargets
		FILE bxTargets.cmake
		NAMESPACE bx::
		DESTINATION lib/cmake/bx
	)
endif()
