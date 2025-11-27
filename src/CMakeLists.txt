file(GLOB_RECURSE BX_SOURCES "*.cpp")

# Add library target
add_library(bx ${BX_SOURCES})

# Ensure MSVC uses the correct preprocessor and language standard
if(MSVC)
	target_compile_options(bx PRIVATE
		/Zc:__cplusplus # Ensure _MSVC_LANG matches __cplusplus
		/Zc:preprocessor # Use the standard conforming preprocessor
	)
endif()

# Define BX_CONFIG_DEBUG and other required macros
target_compile_definitions(bx PRIVATE
	$<$<CONFIG:Debug>:BX_CONFIG_DEBUG=1>
	$<$<NOT:$<CONFIG:Debug>>:BX_CONFIG_DEBUG=0>
	BX_CONFIG_CRT_FILE_READER_WRITER=1
	BX_CONFIG_CRT_DIRECTORY_READER=1
)

# Platform-specific definitions
if(WIN32)
	target_compile_definitions(bx PRIVATE
		BX_PLATFORM_WINDOWS=1
		BX_CRT_MSVC=1
	)
elseif(UNIX)
	target_compile_definitions(bx PRIVATE
		BX_PLATFORM_LINUX=1
		BX_CRT_GLIBC=1
	)
endif()

# Include directories
target_include_directories(bx PUBLIC
	$<BUILD_INTERFACE:${BX_INCLUDE_DIR}>
	$<INSTALL_INTERFACE:include>
)
