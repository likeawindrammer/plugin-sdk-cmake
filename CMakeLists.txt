cmake_minimum_required(VERSION 3.16)

project(plugin-sdk-cmake LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)

file(GLOB HEADERS
    plugin-sdk/plugin_sa/*.h
    plugin-sdk/plugin_sa/game_sa/*.h
    plugin-sdk/plugin_sa/game_sa/*/*.h
    plugin-sdk/shared/*.h
    plugin-sdk/shared/game/*.h
    plugin-sdk/hooking/*.h
    plugin-sdk/injector/*.hpp
)

file(GLOB SOURCES
    plugin-sdk/plugin_sa/game_sa/*.cpp
    plugin-sdk/shared/*.cpp
    plugin-sdk/shared/game/*.cpp
    plugin-sdk/hooking/*.cpp
)

add_library(gtasa STATIC ${SOURCES} ${HEADERS})

IF(MINGW)
    target_compile_options(gtasa PUBLIC -fpermissive)
ENDIF(MINGW)

target_compile_definitions(gtasa PUBLIC -DGTASA -D_DX9_SDK_INSTALLED -DRW)
target_compile_features(gtasa PRIVATE cxx_std_20)
target_include_directories(gtasa PUBLIC
    plugin-sdk/shared/
    plugin-sdk/shared/game/
    plugin-sdk/plugin_sa/game_sa/
)
target_link_directories(gtasa PUBLIC
    plugin-sdk/tools/bass/
)
target_sources(gtasa PRIVATE
    plugin-sdk/plugin_sa/game_sa/
    plugin-sdk/shared/game/
)
target_link_libraries(gtasa bass d3dx9)
