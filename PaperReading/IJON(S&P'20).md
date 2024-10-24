# IJON(S&P'20)

>[IEEE Xplore Full-Text PDF:](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9152719)
>
>Code: https://github.com/RUB-SysSec/ijon

## 1. Motivation

>Current fuzzing methods struggle to explore complex state machines. 

单一的coverage-based fuzzing无法处理复杂的，路径依赖相关的Bug.

现有工业界的fuzzing practitioner的做法：

1. 先运行fuzzing一段时间，然后分析code coverage；
2. 分析和改进fuzzing process以提供code coverage (例如，删除目标中的一些很难的部分，改变突变策略或者直接往input corpus中加样本);

--> Manual tuning is required.



## 2. Method

**State Exploration:**

- **Known Relevant State Values:** 如果程序中只有很小一部分状态是interesting的，fuzzer可能在大多数时候都得不到code coverage的反馈，人类若可以分辨这部分state就可以用于指导fuzzer。

<img src="../Figures/image-20241024112839776.png" style="zoom:50%;" />

​	对这个例子而言，迷宫的state有几百个（x,y）组合，但是代码中只有4条分支，所以fuzzer就算获取了大部分的coverage也很难探索到bug。但是分析人员可以将每一种(x,y)视为一种新状态，告知fuzzer，这样就可以遍历所有可能的情况。

<img src="../Figures/image-20241024113833316.png" alt="image-20241024113833316" style="zoom:67%;" />



<img src="../Figures/image-20241024113941767.png" style="zoom:80%;" />

  再如对超级玛丽来说，由于其坐标数量大概是10^6^数量级，完全遍历每个(x,y)并不明智。分析师可以指定x坐标的增加代表着新的状态，这样就可以很好地规避无用的中间结果。



- **Known State Changes：**若程序过于复杂，或者很难分辨哪些变量会与interesting state有关，在这种情况下（没有Known Relevant State Value），人类可以分辨出代码里那部分可能跟这些有关。例如，例如，许多程序会单独处理信息或输入块，处理不同类型的输入块很可能会以不同方式改变状态。这种情况通常发生在使用高度结构化数据的应用程序中，如消息序列或文件格式中的块列表。

<img src="../Figures/image-20241024115052645.png" style="zoom:67%;" />

  例如对于以下protocol处理代码，fuzzer很难生成有效的信息类型进而覆盖所有分支。

<img src="../Figures/image-20241024115320289.png" style="zoom:67%;" />

​	分析人员可以通过使用成功处理的报文类型日志，使fuzzer能够更有效地探索由报文组合产生的不同状态。



- **Missing Intermediate State：**如果以上两种信息都没有，分析人员可以创建人工中间状态来引导fuzzer。一个典型的例子是magic byte checks，AFL及其类似的fuzzer都没有办法很好的处理它。

<img src="../Figures/image-20241024120037718.png" style="zoom: 67%;" />

  这个例子的难点在于，if(lp->hash == hash && strcmp( lp->string, str) == 0)需要在满足第一个条件（哈希值相等）的条件下逐一比较lp->string和str的每个字符是否相同，这点对于随机变异的fuzzer和符号执行来说很困难。

  人类分析者可以有效识别出这是一个一对多的字符串比较（每个lp->string vs. str），即可以将其转化为一系列简单的单一字符串比较来简化操作。





**作者定义了四个primitives:**

>1. 我们允许分析人员选择与解决当前问题相关的代码区域。
>2. 我们允许直接访问 AFL bitmap来存储附加值。Bitmap entries可以直接设置或增加，因此可以改变feedback状态值。
>3. 我们让分析人员能够影响coverage的计算。这就允许相同的edge coverage产生不同的bitmap coverage。这样就能在不同状态下创建更精细的反馈.
>4. 我们引入了一个可以让用户添加hill climbing optimizaiton(贪心)优化的primitive，这样在状态空间太大时就可以为用户提供一个目标。



**IJON实现方式：**

fuzzing process 1--> 人工分析 --> fixed fuzzing process 2 --> get new coverage input --> feed back to process 1

<img src="../Figures/image-20241024122502394.png" style="zoom:67%;" />

`IJON-ENABLE()`和`IJON-DISABLE()`可以允许/禁止当前fuzzing获取代码覆盖率，从而排除一些不想fuzz的代码块。

<img src="../Figures/image-20241024122711865.png" style="zoom:67%;" />

`IJON_INC()`用于增加bitmap中的coverage。例如上图的x若等于5，我们可以用5，文件名和Line number计算出bitmap中对应的index，并增加其值。

<img src="../Figures/image-20241024123004637.png" style="zoom: 67%;" />

`IJON_SET()`可以直接设置bitmap中对应entry的值，如上图在每个循环开始处更新bitmap从而达到让fuzzer探索不同(x,y)组合的目的。

<img src="../Figures/image-20241024123308018.png" style="zoom:67%;" />

如上图，在成功处理msg后，增加了两行代码，其作用是将成功处理的命令索引添加到state_log中，并根据此更新bitmap，这样fuzzer就会更聚焦于被成功处理了的msg上。

<img src="../Figures/image-20241024123723642.png" style="zoom:67%;" />

`IJON_STATE()`的作用是改变coverage的计算方式，其在计算edge coverage时添加了一个虚拟state。当这个虚拟state改变时，任何edge都会触发新的coverage。

在分析协议之前，程序告诉模糊器如果msg中`has_hello`和`has_login`发生了变化, 就对之后的所有edge给予更多coverage。

<img src="../Figures/image-20241024124841814.png" style="zoom:67%;" />

`IJON_MAX`会让fuzzer进入hill climbing optimizaiton的模式，其作用是在过大的探索空间中优先找到目标，摒弃冗杂的中间值。`IJON_MAX（y,x）`的作用是在每个y上用登山算法贪心找到最大化x的值。使用多个slot的目的是为了避免陷入死胡同。



## 3. Evaluation