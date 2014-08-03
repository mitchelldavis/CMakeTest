cmake_minimum_required (VERSION 2.6)
project(CPPTests)

set(EXE_NAME main)

#Here we're making sure that we have libuv from https://github.com/joyent/libuv.git built and available.
message(STATUS "This project relies on libuv.  So we need to get it.")
execute_process(COMMAND git clone https://github.com/joyent/libuv.git libuv)
message(STATUS "This project relies on libuv.  So we need to build it.")
execute_process(COMMAND python gyp_uv.py -f make WORKING_DIRECTORY libuv)
execute_process(COMMAND make -C libuv/out)

# Include directories
include_directories(libuv/include)
include_directories(includes)

# Add the uv library to the project
add_library(uv STATIC IMPORTED)
link_directories(${CMAKE_BINARY_DIR}/libuv/out/debug/obj.target)

# Setup the executable
add_executable(${EXE_NAME} src/main.cpp)
target_link_libraries(${EXE_NAME} libuv.a)
