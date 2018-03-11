cmake_minimum_required(VERSION 3.9)
project(llvm-pass)

set(CMAKE_CXX_STANDARD 11)


# demo.bc is compiled from hello.c provided by http://llvm.org/docs/GettingStarted.html#an-example-using-the-llvm-tool-chain
configure_file(demo.bc demo.bc COPYONLY)

# If you are using cmake-based IDE (e.g., CLION) to write your passes.
# You are suggested to uncomment the following three lines, and change them accordingly if necessary.
# set(HOMEBREW "/home/linuxbrew/.linuxbrew/")
# set(llvm_include "${HOMEBREW}/Cellar/llvm/5.0.1/include")
# include_directories(${llvm_include})

add_library(llvm-pass SHARED helloPass.cpp)