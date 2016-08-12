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