---
title: "Profiler and tricks"
collection: learning
categories:
    - miscellaneous
permalink: /learning/miscellaneous/profiler/
excerpt: 'Profiler that analyse and optimize the program.'
date: 2024-5-31
author_profile: true
toc: true
---

参考链接：https://www.ebaina.com/articles/140000004810

# 
## 性能分析工具
感受到训练缓慢时，我们应当检查系统的状态来得知代码的性能被哪些因素限制了。计算，是一个由应用程序（代码）、存储设备（硬盘、内存）、运算设备（CPU、GPU）共同参与的过程，有着非常明显的木桶效应，即其中任意一个环节都有可能成为性能瓶颈的来源。

只有将性能瓶颈定位到 CPU、GPU、I/O 或是代码中，才能开始解决问题。
### htop
htop 是一个跨平台的交互式流程查看器。

htop 允许垂直和水平滚动进程列表，以查看它们的完整命令行以及内存和CPU消耗等相关信息。显示的信息可以通过图形设置进行配置，并且可以交互地进行排序和过滤。与进程相关的任务（例如终止和更新）可以在不输入其 PID 的情况下完成。

从 htop 顶部的信息集合中，可以监视 CPU 和内存的使用情况。

一个好的高性能程序，应当尽可能多地进行异步运算，来充分发挥多核 CPU 的能力。同时，尽量多地使用内存，能够大大提高数据的交流效率。当然，这并不意味着你可以把所有的 CPU 和内存资源耗尽，这将使系统不能够正常调度资源，反而拖累计算。

在所有的 Linux 发行版中，你都可以直接从软件仓库中安装 htop，例如：

```bash
sudo apt install htop # Ubuntu/Debian
sudo yum install htop # RHEL/CentOS
sudo zypper install htop # openSUSE
sudo pacman -Syyu htop # Archlinux/Manjaro
```

### iotop 和 iostat

iotop 是用来监视每个命令所占用的 I/O 情况的命令行应用程序。

iostat 则是属于 sysstat 工具包中的一个组件，可以监视外部存储设备（硬盘）当前的 I/O 情况。

需要注意的是，因为涉及到 I/O 情况的监视，所以以上两款程序均需要 root 权限才能正常运行。

安装命令分别为：
```bash
sudo apt install iotop
sudo apt install syssat
```
使用命令：
```bash
sudo iotop
sudo iostat
```
### py-spy
py-spy 是一个针对 Python 程序的采样分析器。

它使您可以直观地看到 Python 程序正在花费时间，而无需重新启动程序或以任何方式修改代码（可以根据正在运行的程序的PID采样获取当前程序的各个函数运行时间）。 py-spy 的开销非常低：为了提高速度，它是用 Rust 编写的，并且不会在所分析的 Python 程序相同的进程中运行。这意味着对生产 Python 代码使用 py-spy 是安全的。

py-spy 可以生成如下的 SVG 图像（拖拽到浏览器即可观察），来帮助你统计每一个 package、model 甚至每一个 function 在运行时所耗费的时间。

安装`pip install py-spy`

使用方式
```bash
py-spy record -o profile.svg --pid 12345 # 推荐这个
# OR
py-spy record -o profile.svg -- python myprogram.py
```
### nvidia-smi
nvidia-smi帮助管理和监视 NVIDIA GPU 设备。一般来说，GPU 的流处理器使用率越高，就说明 GPU 是在以更高的效率运转的。设备的当前功率也能从侧面反映这个问题。换言之，如果你发现你的流处理器利用率低于 50%，则说明模型没能很好地利用 GPU 的并行能力。

主要看两个指标：功率和内存占用。

-------

## 问题分析与解决思路

### PyTorch Workflow
而首先，我们要了解 PyTorch 的工作流程。

因为 PyTorch 使用 Python 接口，同时在底层调用了相当多的 C 库，所以在使用 PyTorch 时，很多细节对用户是不暴露的。实际上，在常见的训练过程中，用户和 PyTorch 一起，大致完成了以下的步骤：

构建模型。将编写好的模型类实例化为 nn.Module 对象。
准备数据。将训练数据和测试数据进行预处理，然后组织为 Dataloader 的形式，并设置好数据增强方案。
定义 Loss Function 和 Optimizer。
主训练循环。
其中，主训练循环决定了网络经过多少次完整的数据集，即我们常说的 epoch：

从 Dataloader 中提取当前 batch 的数据。一般 Dataloader 中只记录了数据的 index 信息，所以每次训练循环时，对应的数据都会从硬盘被读取到内存，然后再从内存放入显存中，交由 GPU 进行后续步骤。
数据经过模型。
将得到的输出送到 Loss Function 中计算损失，随后进行 backward 求导。
Optimizer 执行梯度下降，更新参数。
一般来说，PyTorch 训练的过程的快慢决定于主训练循环。主循环中的每一步都将被执行上万次乃至几十万次，任何的效率提升都能够带来极大的收益。

### CPU 瓶颈
CPU 和 GPU 的计算特点，决定了它们不同的功用：CPU 具有更高的主频和精度，适合于进行串行任务；GPU 拥有几千到上万个 Stream 核心，可以进行大规模的并行任务。

所以，对于数据增强等没有相互依赖的任务交给 CPU 来进行，很大程度上会拖慢训练的进程。在每次的数据导入时，都会产生一定时间的等待。这是一种非常普遍的 CPU 瓶颈，即将不适合 CPU 的任务交给它来处理。

GPU 瓶颈
如果你熟悉梯度下降 (Gradient Descent) 的原理，那么你一定能够理解 batch size 对训练速度的影响。梯度下降将一个 batch 中的平均梯度作为总体梯度方向的近似，进行一次参数更新。Batch size 越大，那么 GPU 内同时并行计算的数据也就越多，相应的训练速度会有很大的提升。

Batch size 的设定对最终的训练结果有一定的影响，但是在一定范围内的调整并不会产生非常大的扰动。

主流观点中，在不过分影响最终的模型性能的前提下，batch size 的选取以最大化利用显存和流处理器为佳。

### I/O 瓶颈
I/O 瓶颈是最常见、最普遍的训练效率影响因素。

正如上文中所描述的，数据将会在硬盘、内存和显存中不断地转移和复制。不同存储设备的读写速度，可能有几个数量级上的差异。

出现 I/O 瓶颈的标志主要有：

系统 I/O 读写（尤其是硬盘读写）占用过高；
内存占用和 CPU 占用都普遍偏低；
显存占用较高的情况下，流处理器的利用率过低。
将数据预读入内存中、异步进行数据加载都是有效的解决方案。一个简单稳定的方式是直接使用 DALI 库。

NVIDIA Data Loading Library (DALI) 是一个可移植的开源库，用于解码和增强图像、视频和语音，以加速深度学习应用程序。DALI 通过重叠训练和预处理减少了延迟和训练时间，缓解了瓶颈。它为流行的深度学习框架中内置的数据加载器和数据迭代器提供了一个插件，便于集成或重定向到不同的框架。

用图像训练神经网络需要开发人员首先对这些图像进行归一化处理。此外，图像通常会被压缩以节省存储空间。因此，开发人员构建了多阶段数据处理流程，包括加载、解码、裁剪、调整大小和许多其他增强算子。这些目前在 CPU 上执行的数据处理流水线已经成为瓶颈，限制了整体吞吐量。

DALI 是内置数据加载器和数据迭代器的高性能替代品。开发人员现在可以在 GPU 上运行他们的数据处理工作。