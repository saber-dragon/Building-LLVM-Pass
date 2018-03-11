cmake_minimum_required(VERSION 3.9)
project(llvm-pass)

set(CMAKE_CXX_STANDARD 11)
set(HOMEBREW "/home/linuxbrew/.linuxbrew/")

set(llvm_include "${HOMEBREW}/Cellar/llvm/5.0.1/include")

#
configure_file(demo.bc demo.bc COPYONLY)

include_directories(${llvm_include})

add_library(llvm-pass SHARED helloPass.cpp)