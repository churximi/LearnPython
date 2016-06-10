**关键词**：theano安装，搭建theano环境, python, 深度学习
因为需要安装theano，结果发现这又是一个难以安装的python包...虽然网上教程不少，然而鱼龙混杂，试验了各种方法流程，最后总算是弄好了，现在把我的过程总结如下：

#### 安装环境
- **64位win7系统**，显卡：GT 730M，笔记本电脑；
- 已安装Visual Studio 2013 （都说VS2015太新不推荐）；
- 借助**Anaconda**来安装theano，因为Anaconda已经集成了很多有用的python库，如numpy、nose、scipy等，强烈推荐。

#### 安装Anaconda
- 我的版本：python2.7 windows 64-bit
- 下载地址：[https://www.continuum.io/downloads](https://www.continuum.io/downloads "")

#### 安装mingw
- 安装完之后，如果Anaconda目录里没有MinGW文件夹（据说这个跟Anaconda版本有关，有的版本会有，有的版本则没有。由于折腾的步骤太多，我也不确定自己这个版本一开始有没有），则通过如下方法安装mingw：
- 打开cmd，直接输入Anaconda的安装命令（为了避免干扰我已经卸载了系统里其他的python版本）：

```
conda install mingw libpython
```

- 安装成功之后，Anaconda文件夹下就会出现MinGW文件夹。如果这个没有装好，运行测试时会提示存在g++问题。
- 这一步并没有下载其他乱七八糟的mingw安装软件，有很多教程是通过安装mingw软件来实现的。


#### 安装theano
- 通过cmd直接运行以下命令（系统里有多个python版本的注意区分pip）：

```
pip install theano
```

- 不要用什么theano.zip解压到目录底下或者theano_installer_latest.msi之类的方法安装。
- 目前的theano版本：0.8.2

#### 配置环境变量
- 需要根据自己的安装路径来添加
- 在PATH添加（这个在Anaconda安装时可能已经设置了）：

```
D:\Anaconda;D:\Anaconda\Scripts;
```

- 新建变量PYTHONPATH：（用来指明theano的安装目录）

```
D:\Anaconda\Lib\site-packages\theano;
```

- 在cmd的home目录中新建 .theanorc.txt 文件（作为theano的配置文件，注意名字中的第一个“.”，如果已经存在，则直接修改该文件），设置如下内容：
   所谓cmd的home目录：打开cmd时，在>前面的默认路径：
 ![cmd的home目录](http://img.blog.csdn.net/20160610160125643)

```
[global]
openmp=False

[blas]
ldflags =

[gcc]
cxxflags = -ID:\Anaconda\MinGW\include
```

#### theano测试
下面测试theano是否安装成功：

- **测试方法1**
用以下两行代码：

```python
import theano
print theano.config.blas.ldflags
```
![这里写图片描述](http://img.blog.csdn.net/20160610164051315)

没有出错(没有返回值)则说明已经配置成功。

其实单单是import theano不报错就已经谢天谢地了。

- **测试方法2**

或者用下面的指令测试（测试时会有其他错误提示或是warnings，但基本上还能运行的话则说明theano没问题，错误提示可能是有些东西还没安装好）：

```python
import theano
theano.test()
```

运行：

![这里写图片描述](http://img.blog.csdn.net/20160610164306248)

例如我这里提示没有nose-parameterized这个模块，安装方法：

```
pip install nose-parameterized
```

例如会提示PyCUDA的相关错误信息等。

**注意**：测试2必须在cpu下运行，在下面的GPU搭建中，如果配置了theano的device = gpu，则测试2就不能运行了。

- **测试方法3**

通过验证BLAS是否安装成功：由于numpy是以来BLAS的，如果BLAS没有安装成功，虽然numpy也可以安装，但是无法使用BLAS的加速。
验证numpy是否真的成功依赖BLAS编译，用以下代码测试：

```
import numpy
id(numpy.dot) == id(numpy.core.multiarray.dot)
```

如果结果为False，表示成功依赖了BLAS加速，如果是TRUE则表示用的是python自己的实现，并没有加速。（我这里总是显示TRUE，暂时不知道为什么，但是前面的测试又表明theano已经安装成功）

---

## 使用GPU
上面的theano配置只是完成了上半部分，这个时候还不能进行gpu加速。如果使用GPU则需要继续以下步骤：

#### CUDA安装
- 首先检查自己的显卡是否支持CUDA（显卡至少是NVIDIA的），在如下网址查看具体是否支持：
  - 查看地址：[http://developer.nvidia.com/cuda-gpus](http://developer.nvidia.com/cuda-gpus)
- 确定已经安装Visual Studio
- 下载对应自己系统版本的CUDA，我用的是CUDA Toolkit 7.5, win7 x86_64
  - 下载地址：[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads "")
- 安装的时候注意选自定义安装，模块全部选上，精简版可能会遗漏需要的模块
- 验证安装成功：cmd输入nvcc -V，如果能出现版本信息，则证明nvcc安装成功

#### Theano文件配置(GPU)
- 编辑Theano的配置文件.theanorc.txt , 添加如下内容:(具体内容应当对应自己的Python版本和VS版本以及路径做适当修改)

```
[nvcc]
fastmath = True
flags = -LD:\Anaconda\libs
compiler_bindir = D:\Microsoft Visual Studio 2013\VC\bin
```

- 继续编辑Theano的配置文件，添加：

```
[global]
device = gpu
floatX = float32
```

####  Theano配置文件
最终的Theano配置文件内容为：

```
[global]
device = gpu
floatX = float32
openmp=False

[blas]
ldflags =

[gcc]
cxxflags = -ID:\Anaconda\MinGW\include

[nvcc]
flags = -LD:\Anaconda\libs
compiler_bindir = D:\Microsoft Visual Studio 2013\VC\bin
```

- 在Python中运行"import theano.sandbox.cuda". 将会编译第一个Cuda文件, 应当没有错误产生。

#### 测试是否使用GPU
- 测试方法1

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
功能：测试是否使用GPU
时间：2016年6月10日 11:20:10
"""

from theano import function, config, shared, sandbox
import theano.tensor as T
import numpy
import time

vlen = 10*30*768  # 10 x cores x threads per core
iters = 1000

rng = numpy.random.RandomState(22)
x = shared(numpy.asarray(rng.rand(vlen), config.floatX))
f = function([], T.exp(x))
print(f.maker.fgraph.toposort())
t0 = time.time()
for i in range(iters):
    r = f()
t1 = time.time()
print('Looping %d times took' % iters, t1 - t0, 'seconds')
print('Result is', r)

if numpy.any([isinstance(x.op, T.Elemwise) for x in f.maker.fgraph.toposort()]):
    print('Used the cpu')
else:
    print('Used the gpu')
```

使用**GPU**的测试结果：1.66秒
![这里写图片描述](http://img.blog.csdn.net/20160610161918447)

使用**CPU**的测试结果：17.28秒

切换成cpu的方法，我是通过更改theano配置文件，将.theanorc.txt的内容只保留以下内容（可能需要重启电脑）：

```
[blas]
ldflags =

[gcc]
cxxflags = -ID:\Anaconda\MinGW\include
```

![这里写图片描述](http://img.blog.csdn.net/20160610162039729)

- 测试方法2

在theano库中找到theano/misc/check_blas.py，运行这个测试文件。
这个测试文件里还有不同型号显卡的性能结果，以供对比。

![这里写图片描述](http://img.blog.csdn.net/20160610162229839)

---

## 其他可选

- Visual Studio 2013下的安装配置
- 为了方便以后建立统一的算法调用平台，故使用Visual Studio来进行图形化界面的开发，安装Python Tools for Visual Studio后即可在Visual Studio环境下来调用当前系统中的Python编译环境。
- 安装Python Tools for Visual Studio （2013），下载地址：
  - [https://github.com/Microsoft/PTVS/releases/v2.2](https://github.com/Microsoft/PTVS/releases/v2.2 "")

---

## 参考文献
- [Theano与CUDA的windows安装](http://www.godeye.org/lesson/116 "")
-  [theano学习笔记(1)环境搭建](http://blog.csdn.net/hjimce/article/details/46654229)
- [Theano 中文安装指南](https://github.com/Theano/Theano/wiki/Theano-Windows-%E4%B8%AD%E6%96%87%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97%28Chinese-Installation-Guide%29)


