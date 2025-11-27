cmake_minimum_required(VERSION 3.16)
project(bx VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(BX_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include" CACHE PATH "Path to bx include directory")

# Options
option(BX_BUILD_TESTS "Build bx tests" OFF)
option(BX_INSTALL "Enable installation of bx library" ON)

add_subdirectory(src)

# Installation
if(BX_INSTALL)
	include(GNUInstallDirs)

	install(TARGETS bx
		EXPORT bxTargets
		LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
		RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
		INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	)

	install(DIRECTORY include/
		DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	)

	install(EXPORT bxTargets
		FILE bxConfig.cmake
		NAMESPACE bx::
		DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/bx
	)
endif()

# Build tests
if(BX_BUILD_TESTS)
	enable_testing()
	add_subdirectory(tests)
endif()

# Export build tree for development
export(EXPORT bxTargets
	FILE "${CMAKE_BINARY_DIR}/bxConfig.cmake"
	NAMESPACE bx::)
