---
layout: post
title: 一篇文章带你零基础入门$NumPy$和$PyTorch$
categories: [PyTorch, DL, Bioinformatics]
description: 生物信息学exp1实验报告
keywords: Bioinformatics, PyTorch, DL
---

在本篇文章里，我们将使用 `NumPy` 和 `PyTorch` 编程，围绕距离矩阵这个问题展开，通过比较运行速度进一步探索循环与矩阵运算在深度学习里的应用。

你可以学习到：`notebook` 的使用技巧，`NumPy` 和 `PyTorch` 的编程入门，循环与矩阵运算的时间比较，GPU加速。
<!-- ======= -->

假设我们有一个任务：给定你一些（假设是 `N` 个）3 维的点，以二维矩阵 `(N, N)` 的形式返回这些点两两间的距离。

方便起见，我们直接启动notebook，并敲上以下代码：

```python
import numpy as np
N = 200
coord = np.random.rand(N, 3)
```

这里我们直接使用 `NumPy` （习惯在代码中简称为 `np` ）的随机数库函数，生成维度为 `(N, 3)` 的矩阵，其中 `N` 的值设置为 `200` 。

> Tips：类似的随机数库都提供了多种分布的随机数方法，比如均匀分布、高斯分布等，可以去网上查阅资料寻找自己想要的随机数类型。

然后我们思考怎么做才能得到这一堆点的距离矩阵。

有过一点编程基础的人很快就想到使用循环处理，没错，这个问题可以很简单的使用二重循环求解：

```python
def loop(coord):
    dist = np.empty([N, N], dtype=np.float32)
    for i in range(N):
        for j in range(N):
            dist[i, j] = np.sqrt(np.sum(np.square(coord[i] - coord[j])))

%timeit loop(coord)
```

我们重点来看代码中的 `loop` 函数。

首先创建一个 `(N, N)` 的矩阵，注意 `empty()` 与 `zeros()` 等函数不同，它创建的矩阵不会设置初始值（即保留内存中的数据），我们后续需要给矩阵一一赋值，因此不用初始值。`dtype` 指定为 `np.float32` ，因为我们的随机初始化的矩阵内部都是浮点数。

简单的两重循环后，我们可以得到想要的答案，运行这个函数，并加上 `%timeit` 的装饰器以计算时间.

得到输出：

```bash
241 ms ± 6.14 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

接下来，我们尝试使用矩阵的广播机制重新做一下：

```python
def broadcast(coord):
    dist = np.sqrt(np.sum(np.square(coord[:, None, :] - coord[None, :, :]), axis=-1))
%timeit broadcast(coord)
```

广播机制没有想象中的复杂，但是代码写起来仍然需要一定的功底，比如说以上代码初学者就不一定写出来。

> Tips：使用 `None` 可以方便的扩充一个维度，相当于 `squeeze` 方法。

读者可以自行查看运算的正确性，我们重点看一下时间消耗：
```bash
1.45 ms ± 14.4 μs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```

我们得到了一个相当惊人的结果，使用循环和单纯的矩阵运算，速度竟然相差如此巨大！

不过这还没完，`NumPy` 虽然是常用的矩阵运算库，但是它并不支持并行计算，如果我们手上有显卡，我们当然想要发挥 `GPU` 的强大运算能力，因此，我们使用 `PyTorch` 重新跑一遍。

> Tips：据我所知，`NumPy` 有对应的 `CUDA` 版本，可以使用 `GPU` 加速，有兴趣的读者可以自行查找相关资料。

相关的代码思想我们已经理解过一遍了，因此直接将其转成 `PyTorch` 版本代码：

```python
import torch as pt
N = 200
coord = pt.rand(N, 3)
pt.cuda.set_device(0)
```

在这里，我们生成随机数据并指定了一块显卡，这意味着我们“似乎”可以使用 `GPU` 来加速计算了。

首先使用双重循环的思想跑一下：

```python
def loop(coord):
    dist = np.empty([N, N], dtype=np.float32)
    for i in range(N):
        for j in range(N):
            dist[i, j] = pt.sqrt(pt.sum(pt.square(coord[i] - coord[j])))
%timeit loop(coord)
```

得到输出：

```bash
682 ms ± 10.8 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

接着使用矩阵运算：

```python
def broadcast(coord):
    dist = pt.sqrt(pt.sum(pt.square(coord[:, None, :] - coord[None, :, :]), dim=-1))
%timeit broadcast(coord)
```

得到输出：
```bash
764 μs ± 201 μs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```

根据输出我们有2个发现。首先，使用了 `GPU` 的 `PyTorch` 代码在双重循环中运行速度反而不如没有 `GPU` 的 `NumPy`代码。这是正常的，因为我们将其拆分为了一个个循环，显卡的并行化自然收到了限制。

其次，使用了 `GPU` 的 `PyTorch` 代码在矩阵运算中运行速度远远超过了没有 `GPU` 的 `NumPy`代码。我们在以后的代码中也应该尽可能地多写这种类型的代码，避免循环。

OK，现在做个总结吧。

如今谈到 `Python` 与 `Deep Learning` 我们经常将其联系在一起，却忽略了为什么是这两者结合在了一起，甚至初学者会认为这是自然而然的，他们并没有搞明白背后真正需要了解的事情。

我们都知道 `Python` 虽然好用，但它的速度实在是太慢了 -- 这其实算是一种误解。作为一种解释型语言，`Python` 需要逐行运行，但是它和其他的语言一样，在解释运行时会自动进行代码优化。

这意味着，如果我们能把更多的语句写在一行里，那么代码将得到更大的优化，就好比本文中我们将双重循环改为了矩阵广播，我们对于速度的改进主要体现在两个地方，一个是 `GPU` 的使用，另一个，就是没有使用循环。所以，我们才能看到跑出的结果为什么是这样的，而不是其他样子的。
