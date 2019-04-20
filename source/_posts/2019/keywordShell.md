---
title: shell翻车记录
comments: false
reward: true
date: 2019-04-17 12:02:56
tags:
  - shell
  - 翻车
categories:
  - 编程
---

```shell
记录头秃的shell翻车记录~
```

<!-- more -->

# 命令翻车

## 逐行读取注意

当我使用逐行读取这个功能时  
第一个百度到的是

```bash
cat xxx.txt | while read line;do
done
```

然而没有想到我曹  
这个管道符`|`居然是一个子 shell  
也就是说外面的全局变量穿进去是不会改变的  
结束子进程不会影响父进程的变量  
我勒个擦

最后费了九牛二虎之力查到了一个合适的  
这个才是不会创造子进程 shell 的终极法宝

```bash
while read line;do
done <xxx.txt
```

## TODO:getopt和getopts

# 命令词汇

## seq

常规命令:输出从 1 到 n 的整数数列

```shell
$ seq 3
1
2
3
```
