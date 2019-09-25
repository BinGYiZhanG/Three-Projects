### 一维插值

#### [scipy.interpolate.interp1d函数介绍](https://docs.scipy.org/doc/scipy-0.19.1/reference/generated/scipy.interpolate.interp1d.html)

#### 插入一个一维函数
* ```x```和```y```是数组值被用来估计一些函数```y=f(x)```,该类返回一个函数，该函数的调用方法使用插值函数来查找新点的值。
* 注意，使用输入值中的NaNs调用interp1d会导致未定义的行为。

#### 参数：
* ```x```:一个一维数组的真实值
* ```y```:一个N维数组的真实值

```py
# -*-coding:utf-8 -*-
import numpy as np
from scipy import interpolate
import pylab as pl

x=np.linspace(0,10,11)
#x=[  0.   1.   2.   3.   4.   5.   6.   7.   8.   9.  10.]
y=np.sin(x)
xnew=np.linspace(0,10,101)
pl.plot(x,y,"ro")

for kind in ["nearest","zero","slinear","quadratic","cubic"]:#插值方式
    #"nearest","zero"为阶梯插值
    #slinear 线性插值
    #"quadratic","cubic" 为2阶、3阶B样条曲线插值
    f=interpolate.interp1d(x,y,kind=kind)
    # ‘slinear’, ‘quadratic’ and ‘cubic’ refer to a spline interpolation of first, second or third order)
    ynew=f(xnew)
    pl.plot(xnew,ynew,label=str(kind))
pl.legend(loc="lower right")
pl.show()

```



