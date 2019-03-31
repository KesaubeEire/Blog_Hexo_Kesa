---
title: ShellIsGood
comments: false
reward: true
date: 2019-03-31 10:50:11
tags:
  - Shell
categories:
  - 编程
---

```shell
- 终于搞出来了Shell的这个项目
```

<!-- more -->

# 初衷

本来就是想搞一个小小的登录程序  
没有想到最后搞了这么大个项目  
浪费了好几天的时间  
不过经此一役我算是完全认识到了 shell 有多么傻逼  
我他妈再用 Shell 我就是傻逼  
python 经过对比真是好用到爆

要不要考虑迁移一下项目到 Python...嗯...让我想想

## 项目需求

- 将账号和程序分离为不同的文件:以便传播
- 将掉线数据统计出 Json 或者怎么着搞进去
- 将 Docker 的内容和数据接口整合
  - 基本完整架构 等待整合
- ipv6 的修复部分
  - 仅针对 Kesa 本机修改
- 解决 curl 单个网址时间过长的问题
  - 想办法实现超过指定时长没有返回值强制 curl 下一个池中网址的定时器

## 项目改动

- 将 www.baidu.com 从网址池中删除:总是看到这货卡住,应该是偶尔被封了

## 整体结构

```C
- Auto_LogiLib.sh     -> 主程序
- LoginTest_Docker.sh -> Docker 启动器
- LoginTest_Parser.sh -> Log 更新器
```

![ojbk.png](http://202.204.62.219:6041/images/2019/03/31/ojbk.png)