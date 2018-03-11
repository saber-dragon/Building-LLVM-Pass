# Writing an LLVM Pass (without Compiling from LLVM Source Codes) 

This repo provides a simple way to build an [LLVM pass](http://llvm.org/docs/WritingAnLLVMPass.html). 
Although we only demonstrate home to build a pass from [helloPass.cpp](helloPass.cpp) which is copied from [Writing an LLVM Pass](http://llvm.org/docs/WritingAnLLVMPass.html), the approach should be adaptable to any other LLVM passes.

## Install LLVM & cmake

```bash
brew install llvm
brew install cmake
```

If you do not install `brew`, please refer to [Homebrew](https://brew.sh/) if you use MacOS, or [Linuxbrew](http://linuxbrew.sh/) if you use Linux.

## Build Your Pass

```bash
export HOMEBREW=/home/linuxbrew/.linuxbrew/
mkdir build && cd build
cmake .. -DCMAKE_C_COMPILER="$HOMEBREW/bin/clang" -DCMAKE_CXX_COMPILER="$HOMEBREW/bin/clang++" -DCMAKE_CXX_FLAGS="`$HOMEBREW/bin/llvm-config --cxxflags`"
make
```

Note that 1) If clang/clang++ is your default C/C++ compiler, you do not need to add `-DCMAKE_C_COMPILER="$HOMEBREW/bin/clang" -DCMAKE_CXX_COMPILER="$HOMEBREW/bin/clang++"`; 3) Make sure your cmake version is at least 3.9.


If everything goes well, you should see similar outputs as shown belows.

> After `cmake`:
```text
BREW/bin/clang++" -DCMAKE_CXX_FLAGS="`$HOMEBREW/bin/llvm-config --cxxflags`"
-- The C compiler identification is Clang 5.0.1
-- The CXX compiler identification is Clang 5.0.1
-- Check for working C compiler: /home/linuxbrew/.linuxbrew//bin/clang
-- Check for working C compiler: /home/linuxbrew/.linuxbrew//bin/clang -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /home/linuxbrew/.linuxbrew//bin/clang++
-- Check for working CXX compiler: /home/linuxbrew/.linuxbrew//bin/clang++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/lgong/GitHub/coding/llvm-passes/build
```

> After `make`
```text
Scanning dependencies of target llvm-pass
[ 50%] Building CXX object CMakeFiles/llvm-pass.dir/helloPass.cpp.o
warning: unknown warning option '-Wno-maybe-uninitialized'; did you mean '-Wno-uninitialized'?
      [-Wunknown-warning-option]
1 warning generated.
[100%] Linking CXX shared library libllvm-pass.so
[100%] Built target llvm-pass
```

## Use Your Pass

```bash
$HOMEBREW/bin/opt -load ./libllvm-pass.so -hello -analyze demo.bc
```

The sample output is as follows.

```text
Hello: main
Printing analysis 'Hello World Pass' for function 'main':
Pass::print not implemented for pass: 'Hello World Pass'!
```


## Acknowledgments

The [`CMakeLists.txt`](CMakeLists.txt) in this repo is tuned from the [Makefile](https://github.com/goyalankit/compilers-and-llvm/blob/master/spring2014-compilers/assignment2/FunctionInfo/Makefile).