cmake_minimum_required(VERSION 3.15)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_definitions(-std=c++17)

set(CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS, "${CXX_FLAGS}")

project(SDL2Test)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS} src)

add_executable(SnakeGame src/main.cpp src/game.cpp src/controller.cpp src/renderer.cpp src/snake.cpp)
string(STRIP "${SDL2_LIBRARIES}" SDL2_LIBRARIES)

# ==========================================
# 跨平台链接策略 (Cross-Platform Linking)
# ==========================================

if (WIN32)
    # Windows (vcpkg 环境)
    # 必须显式链接 SDL2main 作为入口点，并使用现代化的 Target 写法
    target_link_libraries(SnakeGame PRIVATE SDL2::SDL2main SDL2::SDL2)
else()
    # Linux / WSL (apt 环境)
    # 使用传统的变量方式，Linux 不需要 SDL2main
    target_link_libraries(SnakeGame PRIVATE ${SDL2_LIBRARIES})
endif()