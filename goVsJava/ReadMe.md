# Go和Java的性能对比,真的如此吗？[转]
原文如此:https://www.cnblogs.com/sy270321/p/12191492.html

在这里我想测试下Go的指针性能

我的本地环境如下：

Java : jdk1.8，GoLang ：1.15

i5处理器，16G内存，Windows10系统

下面是测试结果:

循环次数为100000000时:

    Java结果：6150253200 ns = 6150.2532 ms = 6.1 s
    Go(*[]int)结果： 8275775900 ns = 8275.7759 ms = 8.2 s
    Go([]int)结果： 5680610700 ns = 5680.610700 ms = 5.6 s

循环次数为10000时:

    Java结果：14406800 ns = 14.4068 ms 
    Go(*[]int)结果： 997200 ns = 0.9972 ms 
    Go([]int)结果： 0 ns = 0 ms 

这里其实可以看到,我的结果和原文博主测试出来的不太一样,我的Go的性能更强.不过为什么会出现[]int 比*[]int好这么多呢?

使用```go tool compile -S src/Main.go```做下对比,发现[*[]int](./☆[]int.txt)的汇编中代码条数更多,而且多用了一次```runtime.panicIndex()```和```NOP```可能这是原因吧.
不过看评论,使用指针可能会导致内存逃逸,给GC带来一定负担,从而导致时间变慢.这里还需要再研究下