cmake_minimum_required (VERSION 3.17)
project(timeAccumulator)

find_package(Boost 1.75.0)
if(Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
endif()

include(FetchContent)
FetchContent_Declare(
        msgpack
        GIT_REPOSITORY https://github.com/msgpack/msgpack-c.git
        GIT_TAG cpp-3.2.1
)
FetchContent_GetProperties(msgpack)
if(NOT msgpack_POPULATED)
    FetchContent_Populate(msgpack)
    add_subdirectory(${msgpack_SOURCE_DIR} ${msgpack_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

FetchContent_Declare(
        ftxui
        GIT_REPOSITORY https://github.com/ArthurSonzogni/ftxui
        GIT_TAG d48e3decfc8a552c3afdbf2201542da403539281
        )
FetchContent_GetProperties(ftxui)
if(NOT ftxui_POPULATED)
    FetchContent_Populate(ftxui)
    add_subdirectory(${ftxui_SOURCE_DIR} ${ftxui_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

include_directories("${CMAKE_BINARY_DIR}/_deps/msgpack-src/include")
include_directories("${CMAKE_BINARY_DIR}/_deps/ftxui-src/include")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++2a -pthread")

add_executable(timeAccumulator main.cpp liveApplication.cpp liveApplication.h
        mainSystem.cpp mainSystem.h intermediateSystem.cpp intermediateSystem.h
        fileSystem.cpp fileSystem.h language.h language.cpp)

target_link_libraries(timeAccumulator
        PRIVATE ftxui::screen
        PRIVATE ftxui::dom
        PRIVATE ftxui::component
        )
set_target_properties(timeAccumulator PROPERTIES CXX_STANDARD 20)
