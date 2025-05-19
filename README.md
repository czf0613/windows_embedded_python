# 带tkinter的Windows嵌入式python
官网下载的Windows嵌入式python缺了tkinter，所以需要进行一下魔改。
**注意，这个仓库只兼容Windows x64架构，其它架构可以参照这里的实现思路来做**

## 使用方法
跟官方的嵌入式python一样，但是我这里修改了一下`python313._pth`文件，使得它会自动在`../src`路径下寻找模块。有需要的话得修改它。

### 怎么用pip？
直接在嵌入式python里安装pip是不合适的。一般来讲，需要在电脑上提前安装好完整的python，然后用完整python自带的pip，把所需的安装包安装到指定的位置，比如说`pip install --target /path/to/your/environment numpy`

然后，在`python313._pth`文件配置那些库存放的位置就好了。

## 怎么实现添加tkinter依赖？

### 安装原版完整python
首先，你一定得需要安装一个官方原版的完整python。

### 移植tkinter文件夹
然后在这个原版的python包目录中，找到`tkinter`文件夹。这个就是`import tkinter`时马上执行的点。至于这个文件夹想放哪都可以，只要在`python313._pth`文件里配置能找到它的路径就可以。我这里是直接把它放进去`python313.zip`这个压缩包里了。

### 原生库移植
因为tkinter是由别的外部动态链接库提供的，所以需要把跟这个动态库有关的东西也移植过来，一共是三样（可能）东西。

#### 1，_tkinter.pyd
这个相当于是一个桥接文件，告诉python怎么去加载C接口的相关操作。这个放哪都行，只要`python313._pth`文件里配置好都行。我这里就直接丢在根目录了，因为我发现人家官方也是这么干的（

#### 2，tk86t.dll和tcl86t.dll
这个是tkinter的核心库。根据Windows的机制，Windows会自动在exe相同目录下去寻找动态链接库，所以这个东西必须丢在跟exe同目录。

#### 3，（可能有）zlib1.dll
一些新版本的python tkinter可能会依赖zlib，所以，如果官方的python包里面有这玩意，就带上它吧，因为tk86t.dll似乎会依赖这个玩意。

### tcl文件夹
这些是tcl模板文件，从官方原包里面复制出来，放在根目录就行。