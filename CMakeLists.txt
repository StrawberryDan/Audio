cmake_minimum_required(VERSION 3.25)
project(StrawberryAudio)
include(FetchContent)


if (PROJECT_IS_TOP_LEVEL)
	FetchContent_Declare(StrawberryConfig
		GIT_REPOSITORY "https://github.com/StrawberryDan/Config.git"
		GIT_TAG "main")
	FetchContent_MakeAvailable(StrawberryConfig)
	include(StrawberryConfig)
endif ()


# Get OpenALSoft
FetchContent_Declare(OPEN_AL_SOFT
	EXCLUDE_FROM_ALL
	GIT_REPOSITORY https://github.com/kcat/openal-soft.git
	GIT_TAG 1.24.3)
FetchContent_MakeAvailable(OPEN_AL_SOFT)

find_strawberry_library(NAMES Core Codec)
new_strawberry_library(NAME StrawberryAudio SOURCE
	src/Dummy.cpp)
target_link_libraries(StrawberryAudio PUBLIC StrawberryCore StrawberryCodec)