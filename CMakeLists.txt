cmake_minimum_required(VERSION 3.16)

project(plugin-sdk-cmake LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)

file(GLOB HEADERS
    plugin-sdk-mingw/plugin_sa/*.h
    plugin-sdk-mingw/plugin_sa/game_sa/*.h
    plugin-sdk-mingw/plugin_sa/game_sa/*/*.h
    plugin-sdk-mingw/shared/*.h
    plugin-sdk-mingw/shared/game/*.h
    plugin-sdk-mingw/hooking/*.h
    plugin-sdk-mingw/injector/*.hpp
)

file(GLOB SOURCES
    plugin-sdk-mingw/plugin_sa/game_sa/*.cpp
    plugin-sdk-mingw/shared/*.cpp
    plugin-sdk-mingw/shared/game/*.cpp
    plugin-sdk-mingw/hooking/*.cpp
)

add_library(gtasa STATIC ${SOURCES} ${HEADERS})

IF(MINGW)
    target_compile_options(gtasa PUBLIC -fpermissive)
ENDIF(MINGW)

target_compile_definitions(gtasa PUBLIC -DGTASA -D_DX9_SDK_INSTALLED -DRW)
target_compile_features(gtasa PRIVATE cxx_std_20)
target_include_directories(gtasa PUBLIC
    plugin-sdk-mingw/shared/
    plugin-sdk-mingw/shared/game/
    plugin-sdk-mingw/plugin_sa/game_sa/
)
target_link_directories(gtasa PUBLIC
    plugin-sdk-mingw/tools/bass/
)
target_sources(gtasa PRIVATE
    plugin-sdk-mingw/plugin_sa/game_sa/
    plugin-sdk-mingw/shared/game/
)
target_link_libraries(gtasa bass d3dx9)
