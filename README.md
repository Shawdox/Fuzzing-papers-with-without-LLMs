# Fuzzing without&with LLMs Papers

Until Oct. 22th 2024.

## 1. Survey & Book

- Xiaogang Zhu, Sheng Wen, Seyit Camtepe, and Yang Xiang. 2022. Fuzzing: A Survey for Roadmap. ACM Comput. Surv. 54, 11s, Article 230 (January 2022), 36 pages. https://doi.org/10.1145/3512345
- Detailed tutorial book by Andreas Zeller: [The Fuzzing Book](https://www.fuzzingbook.org/)



## 2. Binary-only/Black-box Fuzzing

#### 2.1 Code Coverage:

- What is coverage: [Code Coverage Explained: How It Helps Devs and Hackers · seeinglogic blog](https://seeinglogic.com/posts/code-coverage-explained/)

- Get coverage from binaries: [5 Ways To Get Code Coverage From a Binary, From Mundane to Arcane · seeinglogic blog](https://seeinglogic.com/posts/getting-code-coverage/#how-to-get-code-coverage-with-dynamorio-and-drcov) 

- How does current fuzzing methods get coverage: 
  - AFL: 
    
    - Source available: 
    
      AFL-LTO (collision free): [AFLplusplus/instrumentation/README.lto.md at stable · AFLplusplus/AFLplusplus](https://github.com/AFLplusplus/AFLplusplus/blob/stable/instrumentation/README.lto.md)
    
      AFL++ Context sensitive Branch coverage: [AFLplusplus/instrumentation/README.llvm.md at stable · AFLplusplus/AFLplusplus](https://github.com/AFLplusplus/AFLplusplus/blob/stable/instrumentation/README.llvm.md#6-afl-context-sensitive-branch-coverage)
    
      AFL++ N-Gram Branch coverage: [AFLplusplus/instrumentation/README.llvm.md at stable · AFLplusplus/AFLplusplus](https://github.com/AFLplusplus/AFLplusplus/blob/stable/instrumentation/README.llvm.md#7-afl-n-gram-branch-coverage)
    
    - Binary-only: 
    
      [AFLplusplus/docs/fuzzing_binary-only_targets.md at stable · AFLplusplus/AFLplusplus](https://github.com/AFLplusplus/AFLplusplus/blob/stable/docs/fuzzing_binary-only_targets.md)
    
      [AFLplusplus/qemu_mode/README.md at stable · AFLplusplus/AFLplusplus](https://github.com/AFLplusplus/AFLplusplus/blob/stable/qemu_mode/README.md)
    
      Binary rewriting without CFG anlysis: [GJDuck/e9afl: AFL binary instrumentation](https://github.com/GJDuck/e9afl)
    
  - 

#### 2.2 Instrument the Binary:

- Dynamic Binary Instrumentation (DBI) methods:

  - [DynamoRIO](https://dynamorio.org/)
  - [Pin - A Dynamic Binary Instrumentation Tool](https://www.intel.com/content/www/us/en/developer/articles/tool/pin-a-dynamic-binary-instrumentation-tool.html)
  - [Valgrind Home](https://valgrind.org/)
  - [QBDI - QuarkslaB Dynamic binary Instrumentation](https://qbdi.quarkslab.com/)
  - [Frida • A world-class dynamic instrumentation toolkit | Observe and reprogram running programs on Windows, macOS, GNU/Linux, iOS, watchOS, tvOS, Android, FreeBSD, and QNX](https://frida.re/)

- Static rewriting methods:

  - Tools:
    - [dyninst/dyninst: DyninstAPI: Tools for binary instrumentation, analysis, and modification.](https://github.com/dyninst/dyninst)
    - [abenkhadra/bcov: Static instrumentation tool for efficient binary-level coverage analysis.](https://github.com/abenkhadra/bcov)
    - [GJDuck/e9afl: AFL binary instrumentation](https://github.com/GJDuck/e9afl)
    - [GJDuck/e9patch: A powerful static binary rewriting tool](https://github.com/GJDuck/e9patch)
    - [Open Source Software / zafl · GitLab](https://git.zephyr-software.com/opensrc/zafl)
  
  - Tutorial:
    - :star:Binary rewriting and e9patch: [Binary Rewriting without Control Flow Recovery](https://www.youtube.com/watch?v=qK2ZCEStoG0)
  - Papers:
    - ZAFL: [sec21-nagy.pdf](https://www.usenix.org/system/files/sec21-nagy.pdf)
      - Notes: [./PaperReading/ZAFL(Security'21).md](./PaperReading/ZAFL(Security'21).md)
      - Vedio: https://youtu.be/8Z-5aTpk_l0
  
    - 
  
  
  
  

#### 2.3 Binary-only fuzzing framework

- AFL/AFL++: [AFLplusplus/docs/fuzzing_binary-only_targets.md at stable · AFLplusplus/AFLplusplus](https://github.com/AFLplusplus/AFLplusplus/blob/stable/docs/fuzzing_binary-only_targets.md)
- Jackalope: [googleprojectzero/Jackalope: Binary, coverage-guided fuzzer for Windows, macOS, Linux and Android](https://github.com/googleprojectzero/Jackalope)
- TinyInst: [googleprojectzero/TinyInst: A lightweight dynamic instrumentation library](https://github.com/googleprojectzero/TinyInst)

  



## 3. Source/Grey-box Fuzzing

- [Exploring Deep State Spaces via Fuzzing](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9152719)
  - Notes: [./PaperReading/IJON(S&P'20).md](./PaperReading/IJON(S&P'20).md)
- 



## 4. Fuzzing with Large Language Model (LLM)

- [Large Language Model guided Protocol Fuzzing](https://www.ndss-symposium.org/wp-content/uploads/2024-556-paper.pdf)

