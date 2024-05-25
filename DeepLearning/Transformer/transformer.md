# Transformer
## Seq2Seq
![alt text](image.png)

![alt text](image-1.png)

![alt text](image-2.png)

![alt text](image-3.png)

![alt text](image-4.png)

![alt text](image-5.png)

## Seq2seq for Multi-label Classification
diff : Multi-class Classification

### Multi-class Classification 和 Multi-label Classification的区别
![alt text](image-6.png)

![alt text](image-7.png)

# Encoder
![alt text](image-8.png)


![alt text](image-11.png)
![alt text](image-28.png)
![alt text](image-10.png)
![alt text](image-12.png)
![alt text](image-9.png)





**Positional Encoding**

 # Decoder
 ## Autoregressive Decoder
```
One-Hot-Vector
```
![alt text](image-13.png)
![alt text](image-14.png)

![alt text](image-15.png)
![alt text](image-16.png)

![alt text](image-17.png)
![alt text](image-19.png)

![alt text](image-24.png)
![alt text](image-25.png)
![alt text](image-30.png)
+ 上一步的输出是下一步的输入
![alt text](image-31.png)

+ Mask的含义是什么

![alt text](image-32.png)

+ Mask的含义是啥
+ Mask Attention 和 Self-attention的区别是，我们现在不能再看，右边的部分

![alt text](image-33.png)

+ 比如说，我计算q_2我只让他和第一个k_1和k_2计算，而不让他去看k_3,k_4，


![alt text](image-34.png)
+ Why masked?
+ 可是是怎么去实现的呢  
![alt text](image-35.png)


## NAT Decoder
+ 不是自回归的decoder,而是直接输出一整个序列
+ 
![alt text](image-36.png)




![alt text](image-26.png)
## NAT

![alt text](image-27.png)
![alt text](image-29.png)


## Cross Attention
+ Encoder Decoder是如何互动的
![alt text](image-37.png)
![alt text](image-38.png)

# Training
![alt text](image-39.png)
+ inference 就是Testing

![alt text](image-40.png)
+ 我们希望softmax产生的这个概率分布 和 groundTruth的OneHot 编码的向量越接近越好。
+ 
![alt text](image-41.png)
+ 我们希望这四次分类问题 他总和起来的Cross Entropy越小越好

+ 训练的时候Teacher Forcing
+ 在测试的时候Decoder看到的是自己的输入
+ 

# Copy Mechanism
![alt text](image-43.png).
![alt text](image-44.png)

# Guided Attention
## 对ATTENTION增加限制
![alt text](image-45.png)

# Beam Search
![alt text](image-46.png)

# Exposure bias
![alt text](image-47.png)
![alt text](image-48.png)


![alt text](aa7bca133cabdff2e3d6cd41a886c04.png)