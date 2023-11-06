cmake_minimum_required(VERSION 3.25)
project(StrawberryAudio)
include(FetchContent)


# Get OpenALSoft
FetchContent_Declare(OPEN_AL_SOFT
    EXCLUDE_FROM_ALL
    GIT_REPOSITORY https://github.com/kcat/openal-soft.git
    GIT_TAG 1.23.1)
FetchContent_MakeAvailable(OPEN_AL_SOFT)
FetchContent_GetProperties(OPEN_AL_SOFT SOURCE_DIR OPEN_AL_SOFT_SOURCE_DIR)
FetchContent_GetProperties(OPEN_AL_SOFT BINARY_DIR OPEN_AL_SOFT_BINARY_DIR)


# Find and add StraweberryCore
if (NOT TARGET StrawberryCore AND EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/../Core AND IS_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../Core)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../Core ${CMAKE_CURRENT_BINARY_DIR}/StrawberryCore)
elseif(NOT TARGET StrawberryCore)
    message(FATAL_ERROR "Could not find StrawberryCore at ${CMAKE_CURRENT_SOURCE_DIR}/../Core")
endif()


# Find and add StraweberryCordec
if (NOT TARGET StrawberryCodec AND EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/../Codec AND IS_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../Codec)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../Codec ${CMAKE_CURRENT_BINARY_DIR}/StrawberryCodec)
elseif(NOT TARGET StrawberryCodec)
    message(FATAL_ERROR "Could not find StrawberryCodec at ${CMAKE_CURRENT_SOURCE_DIR}/../Codec")
endif()


add_library(StrawberryAudio STATIC
    src/Dummy.cpp)
target_include_directories(StrawberryAudio PUBLIC ${OPEN_AL_SOFT_SOURCE_DIR}/include)
target_link_directories(StrawberryAudio PUBLIC ${OPEN_AL_SOFT_BINARY_DIR})
target_link_libraries(StrawberryAudio PUBLIC StrawberryCore StrawberryCodec OpenAl)

if (NOT ${CMAKE_SYSTEM_NAME} STREQUAL "Darwin")
    target_link_options(StrawberryAudio PRIVATE "-static")
endif()
