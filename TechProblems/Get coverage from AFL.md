# Get coverage from AFL

如何从AFL的结果中获取coverage？

基础的工具：gcov + lcov

## 1. Gcc based

如果目标程序用`afl-gcc`/`afl-gcc-fast`等gcc-based编译的，直接可以使用afl-cov:[Releases · mrash/afl-cov](https://github.com/mrash/afl-cov/releases)



## 2. LLVM based

afl-cov并不支持llvm，因为gcov和lcov都是支持gcc的，对llvm-based的编译器（clang）支持不好。

需要对lcov添加一个warpper，可以参考：[Check Code Coverage with Clang and LCOV | Logan's Note](https://logan.tw/posts/2015/04/28/check-code-coverage-with-clang-and-lcov/)

