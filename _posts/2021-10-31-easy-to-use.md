---
layout: post
title: 一个cpu飙高问题定位的脚本









---

我在[一些故障的排查思路](https://tenwz.github.io/2021/07/09/一些故障排查思路.html)中写过关于 CPU 飙高问题如何分析的文章，但由于其中的步骤说长不长，说短也不短。当我们在定位线上问题时心里难免紧张，如果命令用的不熟，很容易造成定位问题时间过长。

于是在这里造了一个轮子，写了一个只有35行的Shell脚本，思路很简单，将其中几个步骤的结果直接写入jstack后的文件中，一个jstack后的文件即可完成问题定位！

先来看看使用该脚本生成dump文件内容：

![img](https://img-blog.csdn.net/2018021800115355)

有了上图的文件，就可以直接在文件中找cpu占比为94.3的0x65c6等线程的对应代码

为了方便多次采样，该脚本生成的文件名为  pid-$pid-时间.jstack ，默认放在/tmp目录下，比如如果是 ID 为 12345 的Java 进程的 cpu 占比最高，那么通过该脚本生成的文件有可能是 /tmp/pid-12345-20180217T22:51:03.jstack。

附上该shell脚本代码：

```shell
#!/bin/bash

# 入参只有一个，即目标java的pid，如果没有，则默认找cpu最高的java进程
if [ -z "$1" ]; then
        ### 1.先找到消耗cpu最高的Java进程 ###
        pid=`ps -eo pid,%cpu,cmd --sort=-%cpu | grep java | grep -v grep | head -1 | awk 'END{print $1}' `
        if [ "$pid" =  ""  ]; then
                echo "无Java进程，退出。"
                exit
        fi
else
        pid=$1
fi

### 2.生成dump后的文件名 ###
curTime=$(date +%Y%m%dT%H:%M:%S)
# jstack后的文件会加上时间，便于对一个进程dump多次
dumpFilePath="/tmp/pid-$pid-$curTime.jstack"
echo -e "cpu最高的java进程: "`jps | grep $pid`"\n" > $dumpFilePath

### 3.取到该进程的所有线程及其cpu(只显示cpu大于0.0的线程) ###
echo -e "进程内线程cpu占比如下（不显示cpu占比为0的线程）:\n" >> $dumpFilePath
ps H -eo pid,tid,%cpu --sort=-%cpu | grep $pid | awk '$3 > 0.0 {totalCpu+=$3; printf("nid=0x%x, cpu=%s\n", $2, $3) >> "'$dumpFilePath'"} 
END{printf("cpu总占比:%s\n\n", totalCpu) >> "'$dumpFilePath'"}'

### 4.dump该进程 ###
echo -e "如下是原生jstack后的结果:\n" >> $dumpFilePath
jstack -l $pid >> $dumpFilePath

echo "dump成功，请前往查看(文件名包含时间，为了采集更准确，可以多执行几次该命令):" $dumpFilePath

exit
```

记住，执行该脚本你得拥有使用jps、jstack等命令的权限。

使用方法（假设将上述脚本保存为javacpu.sh文件）

方法1：直接让脚本来查找cpu占比最高Java进程并在/tmp目录下生成dump文件，直接sh ./javacpu.sh即可。

方法2：直接使用该脚本在/tmp目录下形成某个Java进程(比如ID为123456)的dump文件，可以 sh ./javacpu.sh 123456。

先解决问题再去定位问题，这是线上事故处理的第一原则，如果遇到线上cpu飙高问题，先多次执行该脚本以便留下“犯罪”现场，然后再重启或回滚服务，后面再去分析该脚本生成的文件来定位问题。



**本文很短，也没有阐述高深的技术知识，但想和各位分享一句从老大那听来的一句话：将小事做到极致，就是创新。工作如此，人生也如此。**