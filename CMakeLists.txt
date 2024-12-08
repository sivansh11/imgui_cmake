cmake_minimum_required(VERSION 3.10)

project(imgui)

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

add_library(imgui ${SRC_FILES} ${HEADER_FILES})

target_include_directories(imgui
	PUBLIC ./imgui/
)
