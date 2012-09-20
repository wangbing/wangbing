矩阵行之间两两相似


## 利用mapreduce求解文档之间的两两相似性
插入图片
{% img  /path/to/image [width] [height] [title text [alt text]] %}
利用mapreduce如何求文档之间的两两相似

文档向量化
按照向量空间模型(VSM)将文档表示term组成的向量

因此问题就转变成求解一个矩阵中，行向量之间的两两相似。

相似度指标：能够表示成

{% img  /post_images/sim_metric.png [相似度度量 text [alt text]] %}

其中V是term字典

如图中公式所示，求解文档之间的相似度，只需要选择两个文档向量共同的非零分量参与运算。
并且相似度的计算，可以分拆成不同的分量积之和。

思路：
1. 建倒排索引
既然只需要对两个文档向量共同项进行相乘并累加，可以按照倒排索引的思路，将文档向量矩阵变成倒排索引矩阵

Term:[(d1,w1),(d3,w3)...]

2. 
建完倒排索引之后，就可以计算每个Term下，包含该Term的文档两两之间的相似度分量
map输出：
(d1,d2): w1*w2
(d1,d3): w1*w3

shuffle之后，reduce直接累加求和，就求出文档之间的相似度。

{% img  /post_images/pairwise_similarity.png %}

不足：
高频词的影响：
关于这个Term的相似度分量计算时，需要计算这些文档两两之间的weight乘积，如果一个Term出现在很多文档中，那么这个Term的倒排索引上的文档将非常多，n(n-1)/2个，如果n很大，不仅计算性能受影响，还可能导致taskserver内存OOM



a^2


