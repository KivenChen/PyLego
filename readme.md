# PyLego 使用指南

## 提前准备

- Python 2.7，目前只有 NXT 支持
- 项目中有你需要的第三方库，在命令行中分别进入 `pyusb` 以及 `pybluez` 文件夹，并运行 `python setup.py install`
- 根据教程安装第三方库，并配置 USB 驱动，把乐高驱动换成 libusb-win32：https://blog.xuezhonghao.com/2018/08/16/nxt-python.html
- 测试版目前仅支持 USB 调试。蓝牙模块正在最后调试阶段
- 把 core.py 放在你正在写的 python 项目文件夹里面
- **目前(2018-12-10)的 `t` 参数暂未提供支持**


## 开始
使用如下开始

``` python
from core import *
```

以下是默认使用的接口，如果你想改动请到 `core.py` 的 `reset()`中更改，第一次导入 `core` 模块时会调用这个函数

``` python
 L-Motor: PORT_B
 R-Motor: PORT_C
 Light(PORT_3)
 Ultrasonic(PORT_2)
 Touch(PORT_4)
```

此外，我们提供了 `reset()` 函数，如果出现连接错误，请调用它重新连接

## 如何阅读

- 每个函数都有参数
- 每个函数的参数都有默认值
- 如果你不想要默认值，可以改其中一个、一些或者全部

## 运动函数
### `l(r=1, p=100, t=None, b=True)`
向左转的函数，注意是**小写的L**
####  参数解析：
- `p`：马力。整数。默认值是75即75%马力
- `r`：转动的距离(rolls)。（**如果 `t` 被修改，那么`r`的值作废！**）
    - 如果 `r` **>=30**，会被认为是**轮子转动的角度(degree)**。比如 `r=360` 就是让右轮旋转360度（正好一圈），于是左转
    - 否则，会被认为是**轮子转动的圈数(circles of rotation)**
- `t`：转动的时间。（**如果 `t` 被修改，那么`r`的值作废！**）**目前(2018-12-10)的 `t` 参数暂未提供支持，下同**
     - 单位：秒
- `b`: 即 break, 是否转完马上刹车
    - 只能是 True 或者 False
    - 默认值 True


#### 例子
``` python
l() # 75%马力，右轮转1圈，使整体左转
l(3) # 75%马力，右轮转3圈，使整体左转
l(p=80, r=3) # 80%马力，右轮转3圈
l(r=45) # 右轮转45度
l(t=10) # 右轮转10秒
```

### `r(p=75, r=1, t=None, b=True)`
右转的函数。请参考 `l` 的说明

### `f(unlimited=False, r=1, p=100, t=None)`
前进(forward)的函数
#### 参数解析
- `unlimited`：是否无限前进，直到下一个操作发生。要不 True 要不 False
- 其余请参考 `l`
- 开发人员笔记：
- **记得首先停止之前的线程**
#### 例子
```python
f(3) #两边轮子转3圈前进
f(True) # 一直前进直到 stop 被调用
#其余参考 l（）函数

### `b(unlimited=False, r=1, p=100, t=None, b=True)`
参考上一个

### `distance()`
获取一个整数：即超声波传感器的读数
单位 **cm**

### `brightness()`
获取一个整数：光传感器的读数
区间：0 ~ 100

### `hit()`
判断是否撞到物体（**传感器被按下，或者是距离<5cm**）
返回值:
- True：是的，撞到了
- False： 不，~

### `black()`
判断地面是否为黑色。返回值参考上一个。

### `green()`
判断地面是否是绿色。参考上一个

### `white()`
白色

### `sound()`
发出声音**（尚不稳定）**