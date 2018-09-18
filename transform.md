## transform简介
&emsp;&emsp;transform 可以对元素进行变换。其中包含：位移、旋转、偏移、缩放。 transform 可以使用 translate/rotate/skew/scale 的方式来控制元素变换，也可以使用 matrix 的方式来控制元素变换。

&emsp;&emsp;事实上，translate,scale,rotate,skew这些方法都是为了便于开发者使用的一个函数；大家在使用的时候肯定有疑惑，它们能够改变元素运动，这其中的本质是什么呢？
## matrix简介
&emsp;&emsp;线性空间中的运动称之为线性变换，在线性空间里，我们要把一个点运动到任意的另一个点，都可以用线性变换来表示，如何表示呢？当我们在这个线性空间里选择了一个基(所谓基就是能够用来表示空间中所有对象的向量组)，那么我们就可以用向量来描述空间中的任何一个对象，然后用矩阵来描述空间中的变换。而使某个对象发生运动的方法就是用代表运动的矩阵乘以代表对象的那个向量。

&emsp;&emsp;那代表运动的矩阵是怎样的呢？此处只谈论2D。

![image](https://github.com/Vincken/image/raw/master/transform/matrix.png)

&emsp;&emsp;2D 的转换是由一个 3*3 的矩阵表示的，前两行代表转换的值，分别是 a b c d e f，要注意是竖着排的，第一行代表 x 轴发生的变化，第二行代表 y 轴发生的变化，第三行代表 z 轴发生的变化，因为这里是 2D 不涉及 z 轴，所以这里是 0 0 1。

&emsp;&emsp;下面我们来看下这个矩阵怎么和我们所熟悉的变换所对应

[transform写法](http://htmlpreview.github.io/?https://github.com/vincken/transform/blob/master/demo/demo1.html)

[matrix写法](http://htmlpreview.github.io/?https://github.com/vincken/transform/blob/master/demo/demo2.html)

## 缩放 scale(x, y)
scale对应的是矩阵中的 a 和 d，x 轴的缩放比例对应 a，y 轴的缩放比例对应 d。

scale(2, 3)对应的矩阵是
```
[
    2  0  0
    0  3  0
    0  0  1
]
```
## 平移 translate(x, y)
translate对应的是矩阵中的 e 和 f，平移的 x 和 y 分别对应 e 和 f。

translate(20px, 40px)对应的矩阵是
```
[
    1  0  20
    0  1  40
    0  0  1
]
```
## 旋转 rotate(θdeg)
rotate对应的是a，b，c，d四个值

其中a = cosθ，b = sinθ, c = -sinθ,d = cosθ。

rotate(30deg)对应的矩阵是
```
[
    0.866  -0.5  0
    0.5  0.866  0
    0  0  1
]
```
## 变形 skew(αdeg, βdeg)
skew对应的是b, c两个值,
其中b = tanβ，c = tanα。

skew(20deg, 30deg)对应的矩阵是
```
[
    1  0.364  0
    0.577  1  0
    0  0  1
]
```
## 变换结合
如果要得到最终的变换矩阵，就要按顺序把所有变换属性的矩阵进行相乘。

![image](https://github.com/Vincken/image/raw/master/transform/multiply.png)

把上面四个矩阵相乘结果得到
```
[
    1.155  -0.369  40
    3  3.144  120
    0  0  1
]
```
## 点的变换
矩阵可以对点（向量）进行转换，方法是用矩阵乘以该向量。

![image](https://github.com/Vincken/image/raw/master/transform/vector.png)

由此我们也可以得到公式，点和点的转换公式
```
x2 = ax1 + cy1 + e;
y2 = bx1 + dy1 + f;
```
## 应用
水平翻转，垂直翻转的matrix实现
[demo](http://htmlpreview.github.io/?https://github.com/vincken/transform/blob/master/demo/demo3.html)

经过原点的任意直线(y = kx)对称：
求经过两点的直线斜率：
```
(y2 - y1) / (x2 - x1)
```
求两点中心点：
```
x = (x1 + x2) / 2
y = (y1 + y2) / 2
```
结果：
```
a = (1-k*k)/(k*k+1);
b = 2k/(k*k+1);
c = 2k/(k*k+1);
d = (k*k-1)/(k*k+1);
```