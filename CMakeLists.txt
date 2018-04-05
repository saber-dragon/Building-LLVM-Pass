cmake_minimum_required(VERSION 3.9)
project(llvm-pass)

set(CMAKE_CXX_STANDARD 11)

# The following cxxflags seem to be necessary
SET(CMAKE_CXX_FLAGS "-Wall -fno-rtti")



# Detect compiler & make sure clang/clang++ is used
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    # using Clang
    message(STATUS "Your compiler is : ${CMAKE_CXX_COMPILER}")
else()
    message(FATAL_ERROR "Your compiler is : ${CMAKE_CXX_COMPILER}, please use clang/clang++")
endif()

# please make sure `llvm-config` is accessible (i.e., have been added
# to the environment path)
if(LLVM_ROOT_PATH)
    message(STATUS "Got LLVM_ROOT_PATH : ${LLVM_ROOT_PATH}")
    set(LLVM_BIN_PATH "${LLVM_ROOT_PATH}/bin")
else()
    message(STATUS "LLVM_ROOT_PATH was not provided, we assume that all llvm related commands (e.g., llvm-config, and opt) are directedly accessible")
endif()

if (LLVM_BIN_PATH)
    execute_process(COMMAND bash -c "${LLVM_BIN_PATH}/llvm-config --includedir"
            RESULT_VARIABLE result
            OUTPUT_VARIABLE llvm_include)
else()
    execute_process(COMMAND bash -c "llvm-config --includedir"
            RESULT_VARIABLE result
            OUTPUT_VARIABLE llvm_include)
endif()

# check results
if(result)
    message(FATAL_ERROR "llvm-config failed: ${result}")
else()
    message(STATUS "Got llvm_include : ${llvm_include}")
    include_directories(${llvm_include})
endif()


if (LLVM_BIN_PATH)
    execute_process(COMMAND bash -c "${LLVM_BIN_PATH}/llvm-config --libdir"
            RESULT_VARIABLE result
            OUTPUT_VARIABLE llvm_lib)
else()
    execute_process(COMMAND bash -c "llvm-config --libdir"
            RESULT_VARIABLE result
            OUTPUT_VARIABLE llvm_lib)
endif()

if(result)
    message(FATAL_ERROR "llvm-config failed: ${result}")
else()
    message(STATUS "Got llvm_lib : ${llvm_lib}")
    link_directories(${llvm_lib})
endif()

# demo.bc is compiled from hello.c provided by http://llvm.org/docs/GettingStarted.html#an-example-using-the-llvm-tool-chain
configure_file(demo.bc demo.bc COPYONLY)


add_library(llvm-pass SHARED helloPass.cpp)