cmake_minimum_required(VERSION 3.21)

project(discord-rpc)

set(CMAKE_CXX_STANDARD 23)
include(cmake/CPM.cmake)

add_library(${PROJECT_NAME} STATIC src/discord-rpc.cpp src/serialization.cpp)
target_include_directories(${PROJECT_NAME} PUBLIC include)

# Windows has some extra code
if (WIN32)
  target_sources(${PROJECT_NAME} PRIVATE src/platform/windows.cpp)
endif()

CPMAddPackage("gh:fmtlib/fmt#11.2.0")
CPMAddPackage("gh:stephenberry/glaze@5.2.0")
target_link_libraries(${PROJECT_NAME} PRIVATE fmt glaze::glaze)

if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
  set(DISCORD_RPC_BUILD_TESTS ON)
endif()

if (DISCORD_RPC_BUILD_TESTS)
  add_subdirectory(test)
endif()