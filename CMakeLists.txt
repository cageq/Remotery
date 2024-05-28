cmake_minimum_required(VERSION 3.13)
project (remotery VERSION 1.0 )
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_C_STANDARD 11)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

 
if(APPLE)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-unused-function -g  ")
elseif(UNIX)
    message(STATUS ${CMAKE_CXX_COMPILER_ID})
	if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -pthread  -g -O2 -march=native   -Wno-unused-function   -Wno-deprecated-declarations -Wno-unused-private-field  ")
	else()
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -pthread  -g -O2 -Wno-unused-function   -Wno-deprecated-declarations -Wno-unused-private-field -Wl,-rpath,. -Wl,-rpath,bin ")
	endif() 
elseif(WIN32)
	add_definitions(-DNOMINMAX)

	add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
	add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")
endif()


include_directories(lib)


add_library(remotery  lib/Remotery.c) 


add_executable(sample sample/sample.c lib/Remotery.c) 
target_link_libraries(sample m)

add_executable(thread sample/thread.c lib/Remotery.c) 
target_link_libraries(thread m)

add_executable(dump sample/dump.c lib/Remotery.c) 
target_link_libraries(dump m)
