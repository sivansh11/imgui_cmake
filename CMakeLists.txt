cmake_minimum_required(VERSION 3.10)

project(imgui_cmake)

include(FetchContent)

set(fetchcontent_quiet False)

FetchContent_Declare(
	imgui
	GIT_REPOSITORY https://github.com/ocornut/imgui.git
	GIT_TAG ab760992adb66258e65f78bb67380a00113e8996
)
if (NOT imgui_POPULATED)
	FetchContent_MakeAvailable(imgui)
endif()

file(GLOB SRC_FILES ${imgui_SOURCE_DIR}/*.cpp)
file(GLOB HEADER_FILES ${imgui_SOURCE_DIR}/*.h)

if (IMGUI_OPENGL3_BACKEND)
  find_package(Vulkan REQUIRED)
	set(OPENGL3_SRC_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp")
	set(OPENGL3_HEADER_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.h"
                          "${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3_loader.h"
  )
	set(SRC_FILES ${OPENGL3_SRC_FILE} ${SRC_FILES})
	set(HEADER_FILES ${OPENGL3_HEADER_FILE} ${HEADER_FILES})
endif()

if (IMGUI_VULKAN_BACKEND)
  find_package(Vulkan REQUIRED)
	set(VULKAN_SRC_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp")
	set(VULKAN_HEADER_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_vulkan.h")
	set(SRC_FILES ${VULKAN_SRC_FILE} ${SRC_FILES})
	set(HEADER_FILES ${VULKAN_HEADER_FILE} ${HEADER_FILES})
endif()

if (IMGUI_SDL3_RENDERER_BACKEND)
  find_package(Vulkan REQUIRED)
  set(SDL3_RENDERER_SRC_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_sdlrenderer3.cpp")
	set(SDL3_RENDERER_HEADER_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_sdlrenderer3.h")
  set(SRC_FILES ${SDL3_RENDERER_SRC_FILE} ${SRC_FILES})
  set(HEADER_FILES ${SDL3_RENDERER_HEADER_FILE} ${HEADER_FILES})
endif()

if (IMGUI_GLFW_BACKEND)
	set(GLFW_SRC_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp")
	set(GLFW_HEADER_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.h")
	set(SRC_FILES ${GLFW_SRC_FILE} ${SRC_FILES})
	set(HEADER_FILES ${GLFW_HEADER_FILE} ${HEADER_FILES})
endif()

if (IMGUI_SDL3_WINDOW_BACKEND)
  set(SDL3_WINDOW_SRC_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_sdl3.cpp")
	set(SDL3_WINDOW_HEADER_FILE "${imgui_SOURCE_DIR}/backends/imgui_impl_sdl3.h")
  set(SRC_FILES ${SDL3_WINDOW_SRC_FILE} ${SRC_FILES})
  set(HEADER_FILES ${SDL3_WINDOW_HEADER_FILE} ${HEADER_FILES})
endif()

add_library(imgui_cmake ${SRC_FILES} ${HEADER_FILES})

target_include_directories(imgui_cmake
	PUBLIC ${imgui_SOURCE_DIR}/
)

if (IMGUI_IMPL_VULKAN_USE_VOLK)
	target_link_libraries(imgui_cmake
  		PRIVATE volk
	)
endif()

if (IMGUI_VULKAN_BACKEND)
	if (IMGUI_IMPL_VULKAN_USE_LOADER)
		target_compile_definitions(imgui_cmake
			PUBLIC IMGUI_IMPL_VULKAN_USE_LOADER=1
		)
	endif()
	if (IMGUI_IMPL_VULKAN_NO_PROTOTYPES)
		target_compile_definitions(imgui_cmake
			PUBLIC IMGUI_IMPL_VULKAN_NO_PROTOTYPES=1
		)
	endif()
 	if (IMGUI_IMPL_VULKAN_USE_VOLK)
  		target_compile_definitions(imgui_cmake
    			PUBLIC IMGUI_IMPL_VULKAN_USE_VOLK=1
	)
	endif()
  target_link_libraries(imgui_cmake
    PUBLIC Vulkan::Headers
  )
endif()

if (IMGUI_GLFW_BACKEND)
  target_link_libraries(imgui_cmake
    PRIVATE glfw
  )
endif()
