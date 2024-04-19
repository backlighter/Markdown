# Classification

Classification 任务不能当做Regression任务的原因
举例
![alt text](image.png)

## Classification不能当做Regression任务做的原因
```
如右图 

1、远远大于1的点，按照regression进行计算会远远大于1
2、我们期望的是，接近1的为Class 1 接近-1的为Class 2
3、这样导致>>1的数 其实是error的。 最终会得到紫色的分割线
4、原因是regression对于model好坏的定义是不适合用在这个地方的。
5、regression model会惩罚那些值太大的点。

for Example:
比如一个Multiple Class Task:
我们简单的把
Class 1 对应 1
Class 2 对应 2
Class 3 对应 3
其实就是默认，Class 1和Class 2比较接近，Class 2和Class 3比较接近。其实是不正确的。
```
```
同理 Regression任务是否可以当做Classification任务做呢
显然是不能的，正如上面所示，Regression的1,2,3,4,5之间是有序列关系的。
但是，如果你所需要的分类之中没有这种近似 邻近的关系，显然是不行的。
```

![alt text](image-1.png)

![alt text](image-2.png)

![alt text](image-3.png)

![alt text](image-6.png)
## 建议去看下二维高斯
![alt text](image-5.png)

![alt text](image-8.png)
```
使用似然函数L找到 一个Gaussian,他sample出这79个点的概率最大。
```
 
![alt text](image-9.png)
```
通俗易懂讲解协方差的文章
https://zhuanlan.zhihu.com/p/672965170
```

![alt text](image-10.png)
```
μ和Σ是从那71个宝可梦的分布中得到的结果。最终得到fμ,Σ 的分布
```
在低维空间看是重叠在一起，但是在高维也许就是可以分开的。

举个例子：一堆粒子，在3D中沿着Z轴，Z>0.5为一类，Z<0.5为一类。
但是这个3D粒子团，沿着Z轴压缩，为一张2D的图，(如下图所示)，很有可能就是重叠在一起的。但是实际上这些例子在高维空间中是存在分割面的。

![alt text](image-11.png)

## 深度学习基本概念
![alt text](image-12.png)
![alt text](image-13.png)
![alt text](image-15.png)
![alt text](image-14.png)
```
b表示constant 
```

![alt text](image-16.png)


![alt text](image-17.png)

![alt text](image-18.png)
![alt text](image-19.png)

![alt text](image-20.png)

![alt text](image-22.png)

```
把W的每一个row或者每一个column都拿出来，拼成一个很长的向量。
b拼上来，c拼上来，等等。这个长的向量 我们使用一个符号叫做θ来把他表示。
θ1，θ2，θ3，θ4这些参数来自于所有未知的参数。
```
![alt text](image-23.png)

![alt text](image-24.png)
```
计算对L的微分 ，组成一个微分的向量,这个向量就是gradient
```

![alt text](image-25.png)
上图 Θ 代表Learning Rate