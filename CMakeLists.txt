cmake_minimum_required(VERSION 3.10)

project(imgui_cmake)

file(GLOB SRC_FILES imgui/*.cpp)
file(GLOB HEADER_FILES imgui/*.h)

if (IMGUI_VULKAN_BACKEND)
	set(VULKAN_SRC_FILE "./imgui/backends/imgui_impl_vulkan.cpp")
	set(VULKAN_HEADER_FILE "./imgui/backends/imgui_impl_vulkan.h")
	set(SRC_FILES ${VULKAN_SRC_FILE} ${SRC_FILES})
	set(HEADER_FILES ${VULKAN_HEADER_FILE} ${HEADER_FILES})
endif()

if (IMGUI_GLFW_BACKEND)
	set(GLFW_SRC_FILE "./imgui/backends/imgui_impl_glfw.cpp")
	set(GLFW_HEADER_FILE "./imgui/backends/imgui_impl_glfw.h")
	set(SRC_FILES ${GLFW_SRC_FILE} ${SRC_FILES})
	set(HEADER_FILES ${GLFW_HEADER_FILE} ${HEADER_FILES})
endif()

add_library(imgui_cmake ${SRC_FILES} ${HEADER_FILES})

target_include_directories(imgui_cmake
	PUBLIC ./imgui/
	PUBLIC ./
)

if (IMGUI_VULKAN_BACKEND)
	if (IMGUI_IMPL_VULKAN_USE_LOADER)
		target_compile_definitions(imgui_cmake
			IMGUI_IMPL_VULKAN_USE_LOADER=1
		)
	endif()
endif()

if (IMGUI_GLFW_BACKEND)
	if (NOT IMGUI_GLFW_PATH)
		message(FATAL_ERROR "GLFW backend required but path not specified")
	endif()
	target_include_directories(imgui_cmake
		PRIVATE ${IMGUI_GLFW_PATH}
	)
endif()
