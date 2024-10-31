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

##### **Human-in-the-loop/More than code coverage:**

- Human in the loop: [S&P'20: Exploring Deep State Spaces via Fuzzing](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9152719)
  - Notes: [./PaperReading/IJON(S&P'20).md](./PaperReading/IJON(S&P'20).md)
- Survey for challenges of human fuzzing practitioner: [The Human Side of Fuzzing: Challenges Faced by Developers during Fuzzing Activities | ACM Transactions on Software Engineering and Methodology](https://dl.acm.org/doi/10.1145/3611668)
- Coverage for data access: [Security'24: Data Coverage for Guided Fuzzing](https://www.usenix.org/system/files/usenixsecurity24-wang-mingzhe.pdf)
  - Notes: [./PaperReading/Data coverage(Security'24).md](./PaperReading/Data coverage(Security'24).md)

- Follow existing bug/vuln report: [Security'24_sdfuzz.pdf](https://peng-hui.github.io/data/paper/sec24_sdfuzz.pdf)
- Exclude irrelevant code with control and data flow anlysis: [S&P'23 SelectFuzz: Efficient Directed Fuzzing with Selective Path Exploration | IEEE Conference Publication | IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/10179296)
- Multi targets fuzzing: [S&P'24 IEEE Xplore Full-Text PDF:](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10646769)




## 4. Fuzzing with Large Language Model (LLM)

Current LLM-based coverage-based fuzzing methods:

| Paper           | Harness/Driver Generation | Input Generation | Seed Scheduling/Mutation | Bug Triage |
| --------------- | ------------------------- | ---------------- | ------------------------ | ---------- |
| CHATAFL[1]      | ×                         | √                | √                        | ×          |
| Fuzz4ALL[2]     | ×                         | √                | √                        | ×          |
| PromptFuzz[3]   | √                         | ×                | ×                        | ×          |
| CovRL-Fuzz[4]   | ×                         | √                | √                        | ×          |
| mGPTFuzz[5]     | ×                         | √                | ×                        | ×          |
| [6]             | √                         | ×                | ×                        | ×          |
| LLMIF[7]        | ×                         | √                | √                        | ×          |
| ProphertFuzz[8] | ×                         | √                | ×                        | ×          |



[1] NDSS'24 Protocol fuzzing: [Large Language Model guided Protocol Fuzzing](https://www.ndss-symposium.org/wp-content/uploads/2024-556-paper.pdf)

- Notes: [mp.weixin.qq.com/s/rm4feDsQSSgRoeiOnToPnQ](https://mp.weixin.qq.com/s/rm4feDsQSSgRoeiOnToPnQ)

[2] ICSE'24 Fuzz4ALL: [Fuzz4All: Universal Fuzzing with Large Language Models | Proceedings of the IEEE/ACM 46th International Conference on Software Engineering](https://dl.acm.org/doi/10.1145/3597503.3639121)

[3] PromptFuzz: [[2409.14729\] PROMPTFUZZ: Harnessing Fuzzing Techniques for Robust Testing of Prompt Injection in LLMs](https://arxiv.org/abs/2409.14729)

[4] ISSTA'24 CovRL-Fuzz: https://dl.acm.org/doi/abs/10.1145/3650212.3680389

[5] Securfity'24 mGPTFuzz: [From One Thousand Pages of Specification to Unveiling Hidden Bugs: Large Language Model Assisted Fuzzing of Matter IoT Devices | USENIX](https://www.usenix.org/conference/usenixsecurity24/presentation/ma-xiaoyue)

[6] ISSTA'24 [How Effective Are They? Exploring Large Language Model Based Fuzz Driver Generation | Proceedings of the 33rd ACM SIGSOFT International Symposium on Software Testing and Analysis](https://dl.acm.org/doi/abs/10.1145/3650212.3680355)

[7] S&P'24 LLMIF [CSDL | IEEE Computer Society](https://www.computer.org/csdl/proceedings-article/sp/2024/313000a196/1WPcYnhN15u)

[8] ProphertFuzz :[ProphetFuzz: Fully Automated Prediction and Fuzzing of High-Risk Option Combinations with Only Documentation via Large Language Model](https://arxiv.org/pdf/2409.00922)



## 5. Tech problems

1. How to get cov from AFL?

[./TechProblems/Get coverage from AFL.md](./TechProblems/Get coverage from AFL.md)

```c
-fprofile-arcs -ftest-coverage
```

