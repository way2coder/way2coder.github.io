---
title: "Python Debugger"
collection: learning
categories:
    - miscellaneous
permalink: /learning/miscellaneous/pdb/
excerpt: 'Tutorials about basic debug methods for python.'
date: 2024-4-24
author_profile: true
toc: true
layout: compress
---
## Python调试方法：
1. 使用打印语句`print()`
2. 使用断言语句，`assert`的意思是，表达式n != 0应该是True，否则根据   程序运行的逻辑后面的代码肯定会出错  `assert n != 0, 'n is zero!'`
3. 把`print()`替换为`logging`是第3种方式
4. `pdb` 模块 

## Pdb模块
启动Python的调试器pdb，pdb有2种用法：
1. 非侵入式方法（不用额外修改源代码，在命令行下直接运行就能调试）
```bash
python3 -m pdb filename.py
```
2. 侵入式方法（需要在被调试的代码中添加一行代码然后再正常运行代码）
```Python
import pdb;
pdb.set_trace()
breakpoint() # or
```

常见pdb命令：
1. 添加断点
```bash
b 
b lineno
b filename:lineno 
b functionname
```


参数：
filename文件名，断点添加到哪个文件，如test.py
lineno断点添加到哪一行
function：函数名，在该函数执行的第一行设置断点    
{: .notice}
说明：
1. 不带参数表示查看断点设置
2. 带参则在指定位置设置一个断点
{: .notice}

2. 添加临时断点（执行一次后自动删除）
```bash
tbreak
tbreak lineno
tbreak filename:lineno
tbreak functionname
```

3. 设置条件断点
```bash
condition bpnumber condition_
```
当condition_为True的时候，断点bpnumber有效，否则断点无效

3. 清除断点
```bash
cl
cl filename:lineno
cl bpnumber [bpnumber ...]  断点序号bpnumber 断点序号（多个以空格分隔）
```
4. 执行程序
```bash
s 执行下一行（能够进入函数体）
n 执行下一行（不会进入函数体）
r 执行下一行（在函数中时会直接执行到函数返回处）
```
5. 查看源代码
```bash
l 十一行
ll 所有
```
6. 退出调试器
```bash
q 
```
7. 