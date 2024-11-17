cmake_minimum_required(VERSION 3.10)
project(VulkanGraphicsProject)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 17)

# ImGui 경로 및 소스 설정
set(IMGUI_DIR ${CMAKE_SOURCE_DIR}/imgui)
set(IMGUI_SOURCES
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/backends/imgui_impl_vulkan.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
)

# Vulkan 및 GLFW 라이브러리 경로 추가
find_package(Vulkan REQUIRED)
include_directories("/opt/homebrew/include")
link_directories("/opt/homebrew/lib")

# 소스 디렉토리 설정
add_subdirectory(src/Core)          # 엔진 핵심 모듈
add_subdirectory(src/Examples)      # 예제 디렉토리

# VulkanApp 실행 파일 설정
add_executable(VulkanApp src/main.cpp ${IMGUI_SOURCES})
target_include_directories(VulkanApp PRIVATE ${IMGUI_DIR} ${IMGUI_DIR}/backends)

# Vulkan 및 GLFW 링크
if(WIN32)
    target_link_libraries(VulkanApp Vulkan::Vulkan glfw Core Examples)
elseif(APPLE)
    target_link_libraries(VulkanApp Vulkan::Vulkan glfw Core Examples "-framework Cocoa" "-framework IOKit" "-framework CoreVideo")
elseif(UNIX)
    target_link_libraries(VulkanApp Vulkan::Vulkan glfw Core Examples pthread dl)
endif()