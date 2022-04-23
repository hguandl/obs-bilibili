cmake_minimum_required(VERSION 3.10)

project(obs-bilibili-plugin)

option(DISABLE_PLUGINS "Disable building of OBS plugins" ON)
set(DISABLE_PLUGINS ON)

set(DISABLE_UI TRUE)
set(ENABLE_SCRIPTING OFF)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/obs-studio/cmake/Modules/")

add_subdirectory(obs-studio)

include(ObsHelpers)
add_subdirectory(obs-bilibili)
