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

## TODO: getopt 和 getopts

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

## linux 下文件夹的复制、覆盖以及确认问题解决

正常我们使用的`cp`  
其实在`alias`中已经被变掉了(其实我的 zsh 并没有,但是 bash 里有的):

```bash
$ alias | grep cp
alias cp='cp -i'
```

> -i 即交互的缩写方式,  
> 也就是在使用 cp 命令作文件覆盖操作之前,系统会要求确认提示.  
> 这个本来是系统的一个保险措施.  
> 如果有很多文件要复制,觉得一个一个输入 y 确认麻烦的话,可以使用如下方法解决:
> `\cp -f $from.txt $to.txt`  
> 只是在命令前加了一个反斜杠`\`，这样就不会再次确认了，而且只在命令中起作用比较好

## 文件重定向保留换行符

重点在后面的 `| sed '\$G'`上

```bash
echo $( xxx ) | sed '\$G' > xxx.txt
```

## 有关重复行

```bash
# 去除重复行
sort file |uniq
# 查找非重复行
sort file |uniq -u
# 查找重复行
sort file |uniq -d
# 统计
sort file | uniq -c
```

# 命令词汇

## seq

常规命令:输出从 1 到 n 的整数数列

```shell
$ seq n
1
2
3
...
n
```
