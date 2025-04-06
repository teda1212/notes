# Introduction to Transformers

## 1. 简介

不需要为不同的输入数据定制模型（CNN for Image, RNN for sequence）

只需要输入的数据token化后交给Transformer就行了

### 1.1 输入

<img src="images/Introduction to Transformers/image-20241029211717919.png" alt="image-20241029211717919" style="zoom:80%;" />

每一个输入为$D*N$维的矩阵（$N个维数为D的向量$），每一列为一个向量，每一行为一个$Feature$

1. 一句话中每个词可以嵌入为一个向量
2. 一张图片分解成一系列的$patches$，每一个$patch$可以嵌入为一个向量

<img src="images/Introduction to Transformers/image-20241029215348169.png" alt="image-20241029215348169" style="zoom:80%;" />

### 1.2 输出

输入一个矩阵$X^{0}$，输出$D*N$维矩阵$X^{M}$

$x _ { n } = X _ { :, n } ^ { ( M ) }$为第$n$个token的特征表示，可以用来

- 预测第$n+1$个token
- 池化后实现分类
- sequence to sequence 或者 image to image预测

## 2. Transformer块

反复使用Transformer块

$$\begin{equation} X ^ { ( m ) } = t r a n s f o r m e r - b l o c k ( X ^ { ( m - 1 ) } )\tag{1}\end{equation}$$

### 2.1 attention

**Attention**

<img src="images/Introduction to Transformers/image-20241029235122330.png" alt="image-20241029235122330" style="zoom: 50%;" />

如果想用前面的词来预测下一个词，那么$A^{(m)}$就为上三角矩阵，为了防止未来的词影响前面词的特征表示

**Self-Attention**

如何计算注意力矩阵？

$$\displaystyle A _ { n , n ^ { \prime } } = \frac { e x p ( x _ { n } ^ { T } x _ { n ^ { \prime } } ) } { \sum _ { n ^ { \prime \prime } = 1 } ^ { N } e x p ( x _ { n ^ { \prime \prime  } } ^ { T } x _ { n ^ { \prime } } ) }$$

将序列中位置之间的相似性与序列内容本身的相似性混为一谈

做线性变换$Ux_{n}$

$$ \displaystyle A _ { n , n ^ { \prime } } = \frac { e x p ( x _ { n } ^ { T } U ^ { T } U x _ { n ^ { \prime } } ) } { \sum _ { n ^ { \prime \prime } = 1 } ^ { N } e x p ( x _ { n ^ { \prime\prime  } } ^{T}U ^ { T } U x _ { n ^ { \prime } } ) }$$

其中，$U为K*D维矩阵，且K<D$
这样，只需使用输入序列中的部分特征来计算相似度，其他特征则被投射出去，从而将注意力计算与内容分离开来。
然而，这种计算是对称的，可能不利，比如'caulking iron'与'tool'之间应该有很强的相似性（钢铁填缝是工具的一种），但给了'tool'和'caulking iron'之间相似性应该更弱（因为工具里面很少遇见钢铁填缝）。

于是，给出了一种非对称形式：

$$ \begin{equation}\displaystyle A _ { n , n ^ { \prime } } = \frac { e x p ( x _ { n } ^ { T } U _{k} ^ { T } U_{q} x _ { n ^ { \prime } } ) } { \sum _ { n ^ { \prime \prime } = 1 } ^ { N } e x p ( x _ { n ^ { \prime\prime  } } ^{T}U_{k} ^ { T } U_{q} x _ { n ^ { \prime } } ) }\tag{2}\end{equation} $$

其中，$q_{n}=U_{q}x_{n}, k_{n}=U_{k}x_{n}$分别被称为queries和keys, $U_{q}和U_{k}$为两个参数

**Multi-head self-attention**

在自注意力机制中，有一个注意矩阵描述序列中两个位置的相似性。但如果点对在某些 "维度 "上相似，而在另一些 "维度 "上不同，这可能会成为结构中的一个瓶颈。

引入多头注意力机制，

$$ Y ^ { ( m ) } = M H S A _ { \theta } ( X ^ { ( m - 1 ) } ) = \sum _ { h = 1 } ^ { H } V _ { h } ^ { ( m ) } X ^ { ( m - 1 ) } A _ { h } ^ { ( m ) }$$

其中，$$ \displaystyle\left[ A _ { h } ^ { ( m ) } \right] _ { n , n ^ { \prime } } = \frac { e x p ( ( k _ { h , n } ^ { ( m ) } + q _ { h , n ^ { \prime } } ^ { ( m ) } ) } { \sum _ { n ^ { \prime \prime } = 1 } ^ { N } e x p ( ( k _ { h , n ^ { \prime } } ^ { ( m ) } ) ^ { T} q _ { h , n ^ { \prime } }^{(m)} ) }$$

​		$$ q _ { h , n } ^ { ( m ) } = U _ { q , h } ^ { ( m ) } x _ { n } ^ { ( m - 1 ) } \quad a n d \quad k _ { h , n } ^ { ( m ) } = U _ { k , h } ^ { ( m ) } x _ { n } ^ { ( m - 1 ) }$$

矩阵乘法示意图：

<img src="images/Introduction to Transformers/image-20241030105236968.png" alt="image-20241030105236968" style="zoom:50%;" />

其中，$$ \theta = \left\{ U _ { q , h } , U _ { k , h } , V _ { h } \right\} _ { h = 1 } ^ { H }$$为参数，通常$K=D/H$

### 2.2 多层感知机

用多层感知机来进行特征交叉，给定位置$n=1,2,...,N$

$$ x _ { n } ^ { ( m ) } = M L P _ { \theta } ( y _ { n } ^ { ( m ) } )$$

### 2.3 残差连接与层归一化

**残差连接**

直接学习输入到输出之间的残差，比如希望学习目标映射为$H(x)$，可以让网络直接学习残差映射$F(x)=H(x)-x$，此时可以得到目标映射$H(x)=F(x)+x$

即从$x ^ { ( m - 1 ) }$到x ^ { ( m  ) }可以通过学习$r e s _ { \theta } ( x ^ { ( m - 1 )})$得到

$$ x ^ { ( m ) } = x ^ { ( m - 1 ) } + r e s _ { \theta } ( x ^ { ( m - 1 ) } )$$

**归一化**

$$ \tilde { x } _ { d , n } = \frac { 1 } { \sqrt { v a r ( x _ { n } ) } } \left( x _ { d , n } - m e a n ( x _ { n } ) \right) \gamma _ { d } + \beta _ { d } = L a y e r N o r m ( X ) _ { d , n }$$

其中，$$ m e a n ( x _ { n } ) = \frac { 1 } { D } \sum _ { d = 1 } ^ { D } x _ { d , n }$$，$$ v a r ( x _ { n } ) = \frac { 1 } { D } \sum _ { d = 1 } ^ { D } ( x _ { d , n } - m e a n ( x _ { n } ) ) ^ { 2 }$$

<img src="images/Introduction to Transformers/image-20241030115655881.png" alt="image-20241030115655881" style="zoom:50%;" />

**最终形式**

<img src="images/Introduction to Transformers/image-20241030115828184.png" alt="image-20241030115828184" style="zoom: 67%;" />
