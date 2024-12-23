# 探究MapReduce与Spark在分布式环境下的迭代性能对比

## 1. 实验目的

- 探究MapReduce与Spark在分布式环境下的迭代性能和资源消耗对比，探究二者的计算特点与资源利用效率。
- 探究内存分配对于Spark迭代性能的影响。

## 2. 实验方法

1. 配置环境并准备两个数据集：web_Google.txt（875713个网页节点，约70MB），graph_dataset.txt（1000000个网页节点，约840MB）。
2. 分别在MapReduce和Spark上对70MB数据集迭代执行PageRank算法20次，两个框架采用相同的参数。
3. 逐步增大Spark内存分配，在Spark上分别用两个数据集迭代执行PageRank算法20次。
4. 将磁盘读写速度，实际内存使用，CPU占用率等指标绘制成图，分析实验结果，得出结论。

## 3. 实验结果

#### MapReduce与Spark在70MB数据集上运行时长对比

- **MapReduce**: 平均运行时长为736秒。
- **Spark**: 平均运行时长为359秒。

实验结果显示，Spark在运行效率上明显优于MapReduce。我们认为这是因为MapReduce在任务执行过程中涉及大量磁盘读写操作，而Spark主要基于内存计算，避免了大量磁盘I/O，因此运行时间更短。

#### MapReduce与Spark在70MB数据集上CPU占用率、实际内存使用、磁盘读写速度对比

MapReduce的CPU占用率、实际内存使用、磁盘读写速度变化图像：

!([图片1.png](https://github.com/Kid-kit-Kid/-MapReduce-Spark-/blob/main/%E5%9B%BE%E7%89%87/%E5%9B%BE%E7%89%871.png))

Spark的CPU占用率、实际内存使用、磁盘读写速度变化图像：

![图片3](C:\Users\kid\Desktop\图片3.png)

- MapReduce的磁盘写操作达到每秒70000kB/s，而Spark最高只有140kB/s，这证明了MapReduce需要大量磁盘写操作。
- 对于内存的对比并不明显，因此我们对Spark进行了进一步实验。

#### Spark在70MB数据集上在不同内存分配下的运行时长对比

<img src="C:\Users\kid\AppData\Roaming\Typora\typora-user-images\image-20241223194606754.png" alt="image-20241223194606754" style="zoom: 33%;" />

由于70MB数据集过小，即使内存设置为最小值512M，计算时间已经饱和，后续优化效果不够明显。

#### Spark在840MB数据集上在不同内存分配下下的运行时长对比

<img src="C:\Users\kid\AppData\Roaming\Typora\typora-user-images\image-20241223195643183.png" alt="image-20241223195643183" style="zoom:33%;" />

随着内存的增大，Spark运行速率明显提高，显示出内存对Spark性能的显著影响。

#### Spark在840MB数据集上CPU占用率

![图片4](C:\Users\kid\Desktop\图片4.png)

Spark运行时CPU性能运行平稳，都保持在几乎相同且较低的利用率。

#### Spark在840MB数据集上内存使用对比

![图片5](C:\Users\kid\Desktop\图片5.png)

占用内存也并未随着分配内存的增大而产生明显的增加

#### Spark在840MB数据集上磁盘I/O对比

![图片6](C:\Users\kid\Desktop\图片6.png)

- 分配512MB大小的内存时，存在少量的读操作，但随着分配内存的增加，读操作消失。
- 综上说明，Spark能够动态分配内存，当内存不足时就会产生额外的I/O读写。

#### 实验结果总结

- 1、在相同的参数下，Spark的迭代运行速度要快于MapReduce。其原因是Spark主要基于内存计算，避免了大量磁盘I/O ，从而在相同的硬件条件下提供更快的计算速度。
- 2、当内存分配不足时，Spark会增加磁盘溢写频率，从而影响性能。并且随着分配内存的增大，性能明显提升。

## 4. 小组分工

王博海（25%）：配置实验环境，汇报PPT 

林凡钧（25%）：运行实验代码，数据可视化 

刘轶鸣（25%）：运行实验代码，编写readme文件 

苏宇翔（25%）：分析实验数据，制作PPT 

