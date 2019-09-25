[原博客](https://blog.csdn.net/qq_20011607/article/details/81412985)


### 一维插值

#### [scipy.interpolate.interp1d函数介绍](https://docs.scipy.org/doc/scipy-0.19.1/reference/generated/scipy.interpolate.interp1d.html)

#### 插入一个一维函数
* ```x```和```y```是数组值被用来估计一些函数```y=f(x)```,该类返回一个函数，该函数的调用方法使用插值函数来查找新点的值。
* 注意，使用输入值中的NaNs调用interp1d会导致未定义的行为。

#### 参数：
* ```x```:```(N,)```一个一维数组的真实值
* ```y```:```(...,N,...) ```一个N维数组的真实值
* ```kind```: ```str```或者```int```类型
    * ```linear```, ```nearest```, ```zero```, ```slinear```, ```quadratic```, ```cubic``` where ```zero```, ```slinear```, ```quadratic``` and ```cubic```,这是默认顺序
* ```axis```:指定要沿着其插入的y轴。内插默认为y的最后一个轴。
* 还有参数```copy,bounds_error,fill_value,assume_sorted```，这里不再解释


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


### 二维插值

#### [scipy.interpolate.interp2d函数介绍](https://docs.scipy.org/doc/scipy-0.19.1/reference/generated/scipy.interpolate.interp2d.html#scipy.interpolate.interp2d)

#### 插入一个超过2维的网格

* 函数式:```z=f(x,y)```,该类返回一个函数，该函数的调用方法使用样条插值来查找新点的值。
* 如果x和y表示一个规则网格，请考虑使用```rectbivariatesline```.
* 注意，使用输入值中的NaNs调用interp2d会导致未定义的行为。

#### 参数：
* ```x,y```:数组类型,```x```可以指定列坐标；```y```指定行坐标,
    * 例如，``` x = [0,1,2];  y = [0,3]; z = [[1,2,3], [4,5,6]] ```
    * 否则，x和y必须指定每个点的完整坐标，例如:
        * ```x = [0,1,2,0,1,2];  y = [0,0,0,3,3,3]; z = [1,2,3,4,5,6] ```
    * 如果```x,y```是多维的，在使用之前要平滑(flatten)
* ```z```:数组类型,插值函数在数据点上的值。如果z是一个多维数组，则在使用前将其压平。如果x和y指定列和行坐标，则flatten的z数组的长度len(x)*len(y)如果x和y指定每个点的坐标，则长度为len(z) == len(x) == len(y)。

* ```kind```:```{‘linear’, ‘cubic’, ‘quintic’},```,可以选择，```linear```是默认的
* 还有参数```copy,bounds_error,fill_value,assume_sorted```，这里不再解释


```cpp
# -*- coding: utf-8 -*-
"""
演示二维插值。
"""
import numpy as np
from scipy import interpolate
import pylab as pl
import matplotlib as mpl

def func(x, y):
    return (x+y)*np.exp(-5.0*(x**2 + y**2))

# X-Y轴分为15*15的网格
y,x= np.mgrid[-1:1:15j, -1:1:15j]

fvals = func(x,y) # 计算每个网格点上的函数值  15*15的值
print len(fvals[0])

#三次样条二维插值
newfunc = interpolate.interp2d(x, y, fvals, kind='cubic')

# 计算100*100的网格上的插值
xnew = np.linspace(-1,1,100)#x
ynew = np.linspace(-1,1,100)#y
fnew = newfunc(xnew, ynew)#仅仅是y值   100*100的值

# 绘图
# 为了更明显地比较插值前后的区别，使用关键字参数interpolation='nearest'
# 关闭imshow()内置的插值运算。
pl.subplot(121)
im1=pl.imshow(fvals, extent=[-1,1,-1,1], cmap=mpl.cm.hot, interpolation='nearest', origin="lower")#pl.cm.jet
#extent=[-1,1,-1,1]为x,y范围  favals为
pl.colorbar(im1)

pl.subplot(122)
im2=pl.imshow(fnew, extent=[-1,1,-1,1], cmap=mpl.cm.hot, interpolation='nearest', origin="lower")
pl.colorbar(im2)
pl.show()


```


##### ```func()函数数学表达式```:$ (x+y)*e^{-5.0*(x^{2}+y^{2})} $


#### 二维插值的三维展示

```py

# -*- coding: utf-8 -*-
"""
演示二维插值。
"""
# -*- coding: utf-8 -*-
import numpy as np
from mpl_toolkits.mplot3d import Axes3D
import matplotlib as mpl
from scipy import interpolate
import matplotlib.cm as cm
import matplotlib.pyplot as plt

def func(x, y):
    return (x+y)*np.exp(-5.0*(x**2 + y**2))

# X-Y轴分为20*20的网格
x = np.linspace(-1, 1, 20)
y = np.linspace(-1,1,20)
x, y = np.meshgrid(x, y)#20*20的网格数据

fvals = func(x,y) # 计算每个网格点上的函数值  15*15的值

fig = plt.figure(figsize=(9, 6))
#Draw sub-graph1
ax=plt.subplot(1, 2, 1,projection = '3d')
surf = ax.plot_surface(x, y, fvals, rstride=2, cstride=2, cmap=cm.coolwarm,linewidth=0.5, antialiased=True)
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('f(x, y)')
plt.colorbar(surf, shrink=0.5, aspect=5)#标注

#二维插值
newfunc = interpolate.interp2d(x, y, fvals, kind='cubic')#newfunc为一个函数

# 计算100*100的网格上的插值
xnew = np.linspace(-1,1,100)#x
ynew = np.linspace(-1,1,100)#y
fnew = newfunc(xnew, ynew)#仅仅是y值   100*100的值  np.shape(fnew) is 100*100
xnew, ynew = np.meshgrid(xnew, ynew)
ax2=plt.subplot(1, 2, 2,projection = '3d')
surf2 = ax2.plot_surface(xnew, ynew, fnew, rstride=2, cstride=2, cmap=cm.coolwarm,linewidth=0.5, antialiased=True)
ax2.set_xlabel('xnew')
ax2.set_ylabel('ynew')
ax2.set_zlabel('fnew(x, y)')
plt.colorbar(surf2, shrink=0.5, aspect=5)#标注

plt.show()

```












