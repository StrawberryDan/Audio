cmake_minimum_required(VERSION 3.25)
project(StrawberryAudio)
include(FetchContent)


# Get OpenALSoft
FetchContent_Declare(OPEN_AL_SOFT
    GIT_REPOSITORY https://github.com/kcat/openal-soft.git
    GIT_TAG 1.23.1)
FetchContent_MakeAvailable(OPEN_AL_SOFT)


# Find and add StraweberryCore
if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/../Core AND IS_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../Core)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../Core ${CMAKE_CURRENT_BINARY_DIR}/StrawberryCore)
else()
    message(FATAL_ERROR "Could not find StrawberryCore at ${CMAKE_CURRENT_SOURCE_DIR}/../Core")
endif()



add_library(StrawberryAudio
    src/Dummy.cpp)
target_link_libraries(StrawberryAudio PUBLIC StrawberryCore OpenAl)