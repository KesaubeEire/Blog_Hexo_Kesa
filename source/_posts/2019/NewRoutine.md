---
title: Shell正则表达式相关
comments: false
reward: true
date: 2019-04-15 14:07:31
tags:
  - shell
categories:
  - 编程
---

```C#
- 通配符
- 普通正则
- 几种处理字符串的命令 : grep|cut|printf|awk|sed
```

<!-- more -->

## 通配符

一种基础的匹配`文件名`的字符  
作用仅存于在`ls`等命令中匹配文件名

- `*` : 匹配任意内容
- `?` : 匹配任意一个内容
- `[ ]` : 匹配括号中的一个字符

## 普通正则

### 特点

- **模糊**匹配:包含这个条件的行就会全部返回

### 元字符

这里的元字符比起其他语言的正则表达式的元字符要少一些  
可能同种元字符在不同的语言中的正则中的作用也有差别

| 元字符  | 作用                                      |
| ------- | ----------------------------------------- |
| \*      | 前面一个字符匹配 0 次或任意多次           |
| \.      | 匹配除了换行符之外的任意一个字符          |
| ^       | 匹配行首                                  |
| \$      | 匹配行尾                                  |
| \[ \]   | 匹配中括号中指定的任意一个字符            |
| [^]     | 匹配中括号中指定的字符以外的任意一个字符  |
| \\      | 转义符                                    |
| \{n\}   | 表示前一个字符恰好出现 n 次               |
| \{n,\}  | 表示前一个字符至少出现 n 次               |
| \{n,m\} | 表示前一个字符至少出现 n 次,至多出现 m 次 |

## 字符串命令

### grep

_[菜鸟教程链接](http://www.runoob.com/linux/linux-comm-grep.html)_  
把含有范本样式的那一行显示出来

语法:

```shell
grep [-noq] [-e<范本样式>] [文件或目录...]
    -e : +<范本样式> 允许使用普通的正则表达式
    -n : 显示行号
    -o : 只显示匹配部分
    -q : 静默使用不返回
    -v : 取反,即取符合条件行的补集
```

### cut

_[菜鸟教程链接](http://www.runoob.com/linux/linux-comm-cut.html)_  
切分获取列  
有点像`sql`中的`select`

语法:

```shell
cut [-df] [file]
    -d ：自定义分隔符，默认为制表符。
    -f ：与-d一起使用，指定显示哪列。
```

### printf

_[菜鸟教程链接](http://www.runoob.com/linux/linux-shell-printf.html)_  
较为底层的输出命令,echo 也是对 printf 的封装  
需要进行**格式化**的确定,比较难用,但可以配置非常复杂且规整的格式  
作为系统管理呈现 log 比较有用

特别提示:在`awk`命令中可以使用`print`命令减少末尾一个`\n`的输入

语法:

```shell
printf  format-string  [arguments...]
```

示例:

```zsh
$ cat test.txt
--------------
Merry a good man.
There is another country.
I don't have apple.
Don't disturb me ok?
Your honor, my idea.

# 这里使用了四个单词一行,两两制表符隔开的格式化
$ printf '%s \t %s \t %s \t %s \n' $(cat test.txt)
--------------------------------------
Merry 	 a 	 good 	 man.
There 	 is 	 another 	 country.
I 	 don't 	 have 	 apple.
Don't 	 disturb 	 me 	 ok?
Your 	 honor, 	 my 	 idea.
```

### awk

_[菜鸟教程链接](http://www.runoob.com/linux/linux-comm-awk.html)_

据说可以非常复杂的一个操作,甚至被称为一种语言:

> awk 作为一个处理文本文件的语言,是一个强大的文本分析工具. --菜鸟

![awk工作原理](http://www.runoob.com/wp-content/uploads/2018/10/20170719154838100.png)

语法:

```zsh
# 用法1:行匹配语句 awk '' 只能用单引号
awk '[pattern]{action}...' {filenames}
# 完整版: 开头 | 中间 | 结束
awk 'BEGIN{action} [pattern]{action} END{action}...' {filenames}

# 用法2:使用.awk脚本进行匹配(真tm牛逼)
awk -f {awk脚本} {文件名}
```

内建变量(重要的):

- FS:字段分隔符
- \$n:当前记录的第 n 个字段,由 FS 进行分隔
- IGNORECASE:忽略大小写的匹配

```zsh
$ df -h
-------
30G     16Gi    66%     222     4294967057  0%
0Bi     0Bi     0Bi     100%    0           0   100%
0Bi     0Bi     0Bi     100%    0           0   100%

$ cat test.txt | awk '{print $2 "\t" $4}'
-----------------------------------------
16Gi    222
0Bi     100%
0Bi     100%

$ cat test.txt | awk   'BEGIN{print "TypeA\tTypeB\n+++++++++++++"}
                        {print $2 "\t" $4}
                        END{print "+++++++++++++"}'
-----------------------------------------------------------------------------------
TypeA	TypeB
+++++++++++++
16Gi	222
0Bi	100%
0Bi	100%
+++++++++++++
```

### sed

Sed 是一种几乎包括在所有 UNIX 平台（包括 Linux）的轻量级流编辑器。  
sed 主要是用来将数据进行选取、替换、删除，新增的命令。

语法:

```zsh
sed [-hnV][-e<script>][-f<script文件>][文本文件]

参数:
    -e 使用多条命令处理
    -n 或--quiet或--silent 仅显示script处理后的结果。

动作:
    -a 追加
    -c 行替换
    -d 删除
    -i 插入
    -p 打印
    -s 字符串替换
```

案例:

```zsh
$ cat hello.go
package main

import "fmt"

func main(){
    fmt.Printf("hello,world~\n")
}

$ sed -n '3p' hello.go
----------------------
import "fmt"

$ sed  '3i\
text' hello.go
--------------
package main

textimport "fmt"

func main(){
    fmt.Printf("hello,world~\n")
}


$ sed -i  "" "s/package/ppt/g" hello.go
---------------------------------------

$ cat hello.go
--------------
ppt main

import "fmt"

func main(){
    fmt.Printf("hello,world~\n")
}
```

> 写了一天最后搞定了.
> 有点难受,明天加油咯~
