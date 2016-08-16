# Production QC project

## 1 first try

### 1.1 first look of data

- training set: 9580 rows
- test set: 2430 rows

有少量的 missing value, 都出现在 param4 列


final y: 明显偏差的数据分布, 绝大部分都集中在 1 附近 (>0.9)

- 采样技术也可能需要(过采样低值, 在以相对偏差为主要质控时可能很重要)
- 考虑用聚类分析是否为异常值(异常低的 key\_index)

    np.log10(1.001 - training_set.key_index_x).hist(bins=100) ## 整理为更接近正态分布的 y

draft parameters:

- seems more like categorical data.
    - decision tree-based algorithms first

直接用 random forest 效果不佳. 过拟合表现.

初期还需要选择特征, 把基本的参数工程明白.

### 1.2 first submission

50, 10, 20 trees for random forest

只是为了看一下近似没有预测的结果...

然而因数据集更新, 没有采到有效数据.

### 1.3 plan

特征工程, 至少不能再过拟合(欠拟合无所谓). 从评估结果看一下自己的初步情况(其实初期自己写脚本已经足够用了).

## 2 RMSE QC

### 2.1 obj

一定要有自己的评估手段, 加快反馈循环.

### 2.2 工程

评估标准是 RMSE. 

写相关的评估函数, 并与参数归一化整合 `RMSE.ipynb`

### 2.3 初步评估

开始犯了个小错误: 评估的 y, y_pred 没有变换回原始的 [0,1] 区间.

note: 时刻保持对 pipeline 的理解, 必要时画结构图来理解流程, 避免此类问题.

评估 random forest, linear reg 的结果:

~~~
RF:
train 0.0424; test 0.0705

linear reg:
train 0.0691; test 0.0716
~~~

考虑 8.12 评测结果: 最优得分是 0.0675, 在 0.067-0.076 之间.

说明目前的数据优化空间从数值上来看不大. (但这一些微小的差距很可能需要大量的工程才能追上)

然而说实话, 目前没什么优化思路.

可能是因为对数据和业务都不理解吧...

决定先提交一次看看情况, 基本在 QC 之后可以弃坑了...

提交的是新数据集采用不同 RF(树的数量不同) 的结果.


### 2.4 dummy predictor

从目前情况来看, 还有一定的提升空间. 另外近几天实验上能做的事情不多, 于是计划继续分析两三天, 到周四/周五, 再决定周末是否提交.

继续理解数据和预测结果:

- 用 均值 / 全1 作为 dummy predictor, baseline 评估目前的预测水平, 以及可以期望的目标.
    - 目前 RF: train 0.4317 pred 0.7064
    - 用均值预测: train 0.6955 pred 0.7210 (test average 0.7194)
    - 结论: 目前的预测虽然不怎么样, 但比 baseline 显著好.

- 继续通过 plot 等方式理解数据和业务, 寻找可能的思路.
    - 考虑是不是相同的输入参数会有很大的偏差 (即结果方差大)
        - 直接看数据可能是个办法
        - 可能结果更多的和生产过程是否正常有关