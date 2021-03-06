**阅读本文，请大声朗读并背诵。**

不保证链接有效，不保证格式标准，不保证内容完备，不保证答案正确。

[TOC]



# 快速复习大纲

## 传统机器学习算法
- SVM
- LR
- 决策树（CART，ID3，C45）
- xgboost，GBDT，RF，Adaboost
- LDA，PCA
- Kmeans，DBSCAN

感知机，softmax，EM，BP神经网络，朴素贝叶斯，核函数，最大熵等
## 深度学习
- 常用激活函数，Adam等优化算法
- FM，FFM

CNN，RNN，LSTM，，梯度消失（爆炸）等
推荐系统：itemBasedCF，userBasedCF，冷启动，SVD（各种变形）
NLP：TF-IDF，textrank，word2vec(能推导，看过源码)，LCA，simhash
## 常见概念
- 判别式模型与生成式模型
- 模型融合方法
- L1L2正则（Lasso，elestic net）
- 最优化方法（梯度下降，牛顿法，共轭梯度法）

最大似然估计，最小二乘法，熵-交叉熵-KL散度，数据归一化，，无偏估计，F1（ROC，recall，precision等），交叉验证，bias-variance-tradeoff
## 常见问题（具体答案去搜知乎或者百度，最好能在实际项目中总结出来）
- 常见损失函数
- SGD与BGD
- 过拟合原因，以及解决办法
- 如何选择特征
- L1为什么能让参数稀疏，L2为什么会让参数趋于较小值，L1优化方法

如何处理样本非均衡问题
如何处理数据缺失问题
各模型的优缺点，以及适用场

# 机器学习

## 基础模型

### 1. 哪些机器学习算法不需要归一化？
首先，分类算法中*
其次，预测算法中

- 贝叶斯分类：
- 决策树：不需要
- SVM：需要
- KNN（K近邻）：需要
- LR：需要
- NN：不需要
- Adaboosting：需要


## 决策树
### 1.介绍下决策树，说一下属性选择方法
一棵决策树的生成过程主要分为以下3个部分:
- 特征选择：特征选择是指从训练数据中众多的特征中选择一个特征作为当前节点的分裂标准，如何选择特征有着很多不同量化评估标准标准，从而衍生出不同的决策树算法。
- 决策树生成： 根据选择的特征评估标准，从上至下递归地生成子节点，直到数据集不可分则停止决策树停止生长。 树结构来说，递归结构是最容易理解的方式。
- 剪枝：决策树容易过拟合，一般来需要剪枝，缩小树结构规模、缓解过拟合。剪枝技术有预剪枝和后剪枝两种

三种决策树算法：ID3、C4.5和CART
- ID3：使用信息增益，没有剪枝的过程
- C4.5：使用信息增益率，可以对不完整数据进行处理。
  具体操作是：在计算信息增益率的时候，在求和的时候对每个叶子前面加上权重值，即该叶子上的非空值样本比例。
- CART：使用Gini指数，包含后剪枝操作。
### 2.熵、信息增益的那几个公式
### 3.如何预剪枝&后剪枝？
**预剪枝**：通过提前停止树的构建而对树剪枝，一旦停止，节点就是树叶，该树叶持有子集元祖最频繁的类。
1. 定义一个高度，当决策树达到该高度时就停止决策树的生长
2. 达到某个节点的实例具有相同的特征向量，及时这些实例不属于同一类，也可以停止决策树的生长。
3. 定义一个阈值，当达到某个节点的实例个数小于阈值时就可以停止决策树的生长
4. 定义一个阈值，通过计算每次扩张对系统性能的增益，并比较增益值与该阈值大小来决定是否停止决策树的生长。

**后剪枝**：首先构造完整的决策树，允许树过度拟合训练数据，然后对那些置信度不够的结点子树用叶子结点来代替，该叶子的类标号用该结点子树中最频繁的类标记。
1. 错误率降低剪枝：将数据分为测试集和验证集，用验证集的预测结果进行比较
2. 悲观错误剪枝：剪枝前后的错误率
3. 代价复杂度剪枝：代价指在剪枝过程中因子树Tt被叶节点替代而增加的错分样本，复杂度表示剪枝后子树Tt减少的叶结点数
### 4.决策树的优缺点
**决策树的优点：**
- 1 决策树易于理解和解释； 
- 2 能够同时处理数据型和类别型属性；
- 3 决策树是一个白盒模型，给定一个观察模型，很容易推出相应的逻辑表达式；
- 4 在相对较短的时间内能够对大型数据作出效果良好的结果；
- 5 比较适合处理有缺失属性值的样本。

**决策树的缺点：**
- 1 对那些各类别数据量不一致的数据，在决策树种，信息增益的结果偏向那些具有更多数值的特征；
- 2 容易过拟合；
- 3 忽略了数据集中属性之间的相关性。
### 5. 为什么决策树不需要归一化？
因为决策树里面的分类就是按照该属性的取值进行分类，分类点的取值在属性的取值范围内，所以不需要归一化。

## 数据特征处理
### 1. ML项目流程
理解：分为两个部分，特征相关和模型相关。
- 业务问题抽象为数学问题
- 数据获取
- 特征工程
- 模型训练，调优
- 模型验证，误差分析
- 模型融合
### 2. 数据归一化和标准化区别及原因
- 标准化：数据缩放到一个小的特定区间
- 归一化：缩放到(0, 1)之间
### 3. 欧氏距离和曼哈顿距离的区别
- 公式
- 理解：曼哈顿距离就是街区距离，欧氏距离就是直线距离。
- 图解
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181203212415978.png)
### 4. 特征降维有哪些方法？
- 缺失值比率：如果某特征的缺失值比率太低，低于阈值，则去掉该特征。
- 低方差滤波：如果某特征的值方差小，即变化的很小，则去掉。
- 高相关滤波：如果两列的变化趋势相似，只保留一列
- PCA：可以通过正交变换将原始的n维数据集变换到一个新的主成分的数据集中。
- 随机森林：可以输出模型的重要程度。例如使用一个很大但是层次很浅的树，每棵树只训练一部分属性。
- 反向特征消除：先用n个特征，再用n-1个特征。计算去掉每个之后的delta错误率，选择错误率最低的那个。
- 前项特征构造：逐个加新的特征，选择提升最有效的特征。
  [机器学习降维方法总结](https://www.cnblogs.com/-Sai-/p/6868534.html)
### 5. 特征相关性
- 距离：曼哈顿，马氏距离，欧氏距离，切比雪夫距离
- 相关系数：pearson，spearman，kendall（肯德尔）
- 相似度：余弦相似度
### 6. 特征重要性
- 决策树：信息增益，基尼指数
- 随机森林：
  （1）基尼指数
  （2）袋外数据的误差(OOB, out-of-bag)
  袋外数据误差（errOOB1）:每次重复抽样时，使用没有被利用数据进行计算模型的预测错误率。随机对袋外数据加入噪声干扰，再次计算袋外数据误差（errOOB2）。假设森林中有N棵树，特征的重要性 = sum(errOOB1 + errOOB2) / N。加入随机噪声之后，如果准确率大幅度下降（errOOB2上升），说明该特征对于样本的预测结果有很大影响，重要程度高。
  ![d53e4ba87c43e65b0ba93411516b84e5.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p167)
- AdaBoost：AdaBoost在迭代的时候，会修改样本的权重。理论上，使用一层的CART决策树可以计算特征重要性。
- Logistic回归和SVM：特征系数的绝对值可以代表特征的重要性，即自变量x对应的权重w。
  [如何用机器学习算法计算特征重要性](https://blog.csdn.net/nevergiveup_go/article/details/80806565)

## 模型评价
### 1. 什么是过拟合？
模型把训练样本学习得太好了，很可能把训练样本自身的特点当做了测试集可能具有的一般性质，这样会导致模型的泛化程度的降低。
准确度在训练集上很高，在测试集上很低。
### 2. 过拟合的原因？
- 噪声：如果噪声太多，会影响模型的学习
- 样本量较少，模型会过度解读到很多假的但是在这少数几个样本拥有的规律。
- 训练模型过度，模型太复杂
### 3. 过拟合的危害
- 过拟合的泛化能力太差
- 模型过于复杂，会利用一些实际没有用的关系，这会导致错误的结果
### 4. 怎么防止过拟合？
- 清洗数据
- 减少样本
- 简化模型复杂度
- 增加正则项
- droupout（随机失活）
### 5.AUC



## 正则项
彩色的线是等高线。L1范数是线性模型，很容易在角上相交，也就是坐标轴上，在角上相交就很容产生稀疏性。
![e8ef8b77c087f0225343f7833d0d9ee6.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p158)
### 1.为什么L1范数导致稀疏解？
![2db0f40a9cb6faba926fb79bb2f6ac53.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p152)
### 2.为什么L0和L1都可以实现稀疏，而选择L1?
![05b50d476741aae0cd6fd183c2ff5e0b.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p153)
### 3.如何选择L1范数的参数?
![b4d5b6d1c03bcd0441f1248f67896e3f.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p154)
### 4.为什么L2正则化可以获得值很小的参数？
![eb9fc42ef402156347af86c2dfd9c0d5.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p155)
### 5.为什么L2正则化可以防止过拟合？
![bc1f2eb82b03a90356a0c3652bcddebc.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p156)
### 6.L1和L2正则的区别？
![1b4c492a9e9aafee04b9e5a0b07f190d.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p157)
### 7.L1 L2了解吗？
1.L12的公式：绝对值，平方
2.作用，为什么有这样的作用
3.分别的特点：L1的稀疏解


## 优化算法
BGD（所有样本） -> SGD（一个样本） -> Mini-batch GD（batch内样本） 
-> Adagrad（加上上一次） -> Adadelta(学习率分母变成MSE) -> Ada（分子变成delta MSE）
-> Adam（一阶导二阶导的帽为修正）



### 2. LR为什么要对特征离散化？
离散化：连续数值的离散化。
等宽分箱，等高分箱

参考：[特征怎么离散化？为什么需要离散化？](https://blog.csdn.net/iyuanshuo/article/details/79963700)

### 3. LR和SVM的区别
- 都是线性分类器
- 都是判别模型
- SVM不依赖所有点，LR受到所有点的影响，如果数据不同类别处于不平衡状态，需要先对数据进行平衡处理
- SVM需要对数据标准化，因为需要对距离进行规范化；LR不需要
- 损失函数不同，LR是logloss，SVM是hinge loss。
  补充，hinge loss: $f(x) = max(0, 1-x)$

### 4. 核函数相关
- SVM如何选择核函数？
- 为什么其他算法没有用到核函数？
  [【机器学习基础】核函数详解](https://blog.csdn.net/qq_25024883/article/details/84824510)

### 5. Gini指数 vs 熵
- 都可以表示数据的不确定性，不纯度。
- Gini指数的计算不需要对数运算，更加高效。
- Gini指数偏向于连续属性，熵更偏向于离散属性。

# 集成模型
## 概述
- 个体学习器之间存在强依赖关系，必须串行生成的序列化方法*
- 个体学习器之间不存在强依赖关系，可同时生成的并行化方法
  1.[【机器学习基础】决策树及其ensemble方法（RF, LGBM, Xgboost, GBDT, 梯度提升boosting）](https://blog.csdn.net/qq_25024883/article/details/84900635)
## Boosting
降低偏差（bias)
## Bagging
降低方差
## Stacking
将第一层的输出train再结合其他的特征集再做一层，就是stacking。例如gbt+lr

## Random Forest
### 1.原理
在Bagging集成的基础上，进一步在决策树的训练过程中引入了随机特征选择。过程分为四个部分：
- 随机选择样本（bootstrap放回抽样）
- 随机选择特征
- 构建决策树
- 随机森林投票（平均）
### 2.优缺点
（1）缺点：
- 随机森林在分类的效果比回归好。因为RF并不能给出一个连续型的输出。而且预测的时候不能超出数据的范围，可能导致有噪声的数据出现过拟合。
- 忽略属性之间可能存在的相关性
- 无法控制模型内部的运行，只能在不同的参数和随机种子之间进行尝试
  （2）优点:
- 高度并行，易于分布式实现
- 随机森林可以解决分类和回归，方差和偏差都较低，泛化性能比较好
- 对高维数据处理很好，并确定最重要的变量，因此被认为是一个不错的降维方法。【模型能够输出特征的重要性程度】？？？
- 存在分类不平衡时，可以提供平衡数据集误差的方法？
- 由于是树模型，不需要归一化即可直接使用

## GBDT
### 1. GBDT适用范围？
- GBDT 可以适用于回归问题（线性和非线性）；
- GBDT 也可用于二分类问题（设定阈值，大于为正，否则为负）和多分类问题。
### 2. GBDT和随机森林（RF）的区别？
相同点：) 都是多棵树
(2) 最终结构由多棵树共同决定
不同点：
(1) RF的组成可以是分类树、回归树；组成 GBDT 只能是回归树。
(2) RF的树可以并行生成（Bagging）；GBDT 只能串行生成（Boosting）
(3) 对于最终的输出结果而言，RF使用多数投票或者简单平均；而 GBDT 则是将所有结果累加起来，或者加权累加起来；
(4) RF对异常值不敏感，GBDT 对异常值非常敏感；
(5) RF对训练集一视同仁权值一样，GBDT 是基于权值的弱分类器的集成；
(6) RF通过减小模型的方差提高性能，GBDT 通过减少模型偏差提高性能。
### 3. GBDT相较于决策树有什么优点？
泛化性能更好！GBDT 的最大好处在于，每一步的残差计算其实变相的增大了分错样本的权重，而已经分对的样本则都趋向于 0。这样后面就更加专注于那些分错的样本。
### 4. GBDT的gradient体现在哪里？
可以理解为残差是全局最优的绝对方向，类似于求梯度。
### 5. GBDT的re-sample
GBDT 也可以在使用残差的同时引入 Bootstrap re-sampling，GBDT 多数实现版本中引入了这个选项，但是是否一定使用有不同的看法。
原因在于 re-sample 导致的随机性，使得模型不可复现，对于评估提出一定的挑战，比如很难确定性能的提升是由于 feature 的原因还是 sample 的随机因素。

## Xgboost
**不放回抽样**
Xgboost是GBDT的一个变种，最大的区别是xgboost通过对目标函数做二阶泰勒展开，从而更新树的叶子的权重和树的权重，并根据loss function求出每一次分裂节点的损失减小的大小，根据分裂损失选择合适的属性进行分裂。
【源码参考：[XGBoost解析系列--源码主流程](https://blog.csdn.net/matrix_zzl/article/details/78699605)】

### 建树方式
- 和RF相同。在构建树的过程中，对每棵树随机选择一些属性作为分裂属性（build_single_tree的方法类似，即)
```python
features = np.random.randint(0, col-1, col/2)
features = np.unique(features)
fea_list = features.tolist()
```
- xgboost使用exact算法。
### 树分裂方式:exact
两种分裂算法：精确分裂exact，近似分裂approx。
- 精确：把每个属性的每个取值作为阈值进行遍历切割，采用CART决策树。
- 近似：对每个属性的所有取值进行分桶，按照各个桶之间的值作为划分阈值。（spark RF里面使用等频分桶） （spark里面使用sort实现）
```scala
//找到切分点（splits）及箱子信息（Bins）
//对于连续型特征，利用切分点抽样统计简化计算
//对于离散型特征，如果是无序的，则最多有个 splits=2^(numBins-1)-1 划分
//如果是有序的，则最多有 splits=numBins-1 个划分
```
**xgboost的特点**
提出了一种特殊的分桶策略，一般的分桶策略是每个样本的权重都是相同的，但是xgboost使每个样本的权重为损失函数在该样本点的二阶导。
(泰勒展开不应该是损失函数关于模型的展开吗？为什么会有在该样本点的二阶导这种说法？ 因为模型是对所有样本点都通用的，把该样本输入到二阶导公式中就可以得到了)。

*所有建树算法：'auto', 'approx', 'exact', 'hist', 'gpu_exact', 'gpu_hist'等，默认设置为'auto'。使用'auto'自适应到具体的算法，对于数据量小于222222使用'exact'精确方法，否则会重置'approx'近视方法。计算批次量大小max_row_perbatch=min(用户设置max_row_perbatch, safe_max_row)，进行批次处理，其中safe_max_row=216216*


### 树集成方式
- xgboost对每棵树的叶子节点个数和权重做了惩罚，避免过拟合。具体是：
   参数 * 叶子个数N + 参数 * sum(叶子权重 * 叶子上的样本数)

### 分布式
RF的并行化是树与树之间的并行化。
xgboost和boosting方法一样，在树的计算上是串行的，但是在构建树的过程中，也就是在**分裂节点**的时候支持并行化。
比如同时计算多个取值作为分裂特征及其值，然后选择收益最大的特征及其取值对节点分裂。
*一般的feature parallel就是对数据做垂直分割（partiion data vertically，就是对属性分割），然后将分割后的数据分散到各个workder上，各个workers计算其拥有的数据的best splits point, 之后再汇总得到全局最优分割点。*

【源码讲解】
![b134f15814a3caa3da32facca1ea3b84.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p166)
三个函数：
- AddBudget：将每个不为0的值放到多个线程中，一个线程中统计该线程中不同值出现的次数。类似于map
- InitStorage：统计多个线程中值的个数。类似于reduce
- Push：进行排序。类似于shift
  *稀疏矩阵的存储方式：外层一维数组，内层是每个feature不为0的值和不为0的值的偏移量。理解就是，假如有N个不为0的值，一个N✖️2的矩阵，第一行是值，第二行是位置（到0的偏移量）。*

### 稀疏数据
xgboost在计算分裂收益的时候只利用了没有missing值的样本。但是在确定了树的结构的时候！xgboost分别假设该样本属于左子树和右子树，比较两者的分裂增益，选择增益较大的那一边作为该样本的分裂方向。

### level_wise
对每一个叶节点进行增益的计算。
## LGBM
### 树分裂方式
xgboost:level-wise. LGBM:leaf-wise.
xgboost对每一层所有节点进行无差别分裂，可能有些节点的增益非常小，对结果影响不大，但是xgboost也进行了分裂，开销太大了。
leaf-wise是在当前所有叶子节点中选择分裂收益最大的节点进行分裂，如此递归，但很容易过拟合，陷入比较高的深度当中，**所以要对最大深度做限制**，避免过拟合。
### 建树方式:hist
histogram算法在内存和计算代价上都有不小优势。
- 内存上优势：直方图算对特征分桶后只需保存特征离散化之后的值，而xgboost的exact算法既要保存原始feature的值，也要保存这个值的顺序索引。
- 计算上的优势，预排序算法需要遍历所有样本的特征值，,而直方图算法只需要遍历桶就行了。

**为什么xgboost的近似直方图比LGBM的直方图算法慢？**
一个子节点的直方图可以通过父节点的直方图减去兄弟节点的直方图得到，从而加速计算。
xgboost在每一层都动态构建直方图， 因为xgboost的直方图算法不是针对某个特定的feature，而是所有feature共享一个直方图(每个样本的权重是二阶导),所以每一层都要重新构建直方图，而lightgbm中对每个特征都有一个直方图，所以构建一次直方图就够了。

### 分布式
- 特征计算增益
- 数据集的直方图汇总的时候
  常用方式是各个worker做自己的直方图，然后汇总各个worker的直方图得到全局的直方图。LGBM是不汇总所有的直方图，只汇总不同worker的不同feature的直方图。

### 类别特征
在对离散特征分裂时，分裂时的增益算的是“是否属于某个类别的”增益。

## 1. Bagging vs Boosting
1.最主要的区别是取样方式不同。Bagging的训练集的选择是随机均匀的，Boosting的训练集的选择与前面的学习结果有关，所以Boosting的分类精度要优于Bagging。
2.模型的集成方式上，Bagging的各个预测函数没有权重，Boosting有。
3.运行方式上，Bagging可以并行生成各个预测函数，Boosting只能顺序生成。所以针对NN，Bagging可以节省大量时间开销。
【Bagging是降低方差，Boosting是偏差】
理解：
Bagging是多个模型并行集成，并且样本可放回抽样。
Boosting是每一次都在修订前面模型的结果，也就是说在降低错误率，降低偏差。
## 2. Xgboost vs GBDT 
1.xgboost使用了泰勒展开
2.xgboost使用了多线程
3.xgboost在代价函数中加入了正则项，用于控制模型的复杂度。

## 拓：Xgboost vs LGBM
4.LGBM基本原理与Xgboost一样，但是速度更快：
- 分裂方式不同：xgboost是level-wise，GBDT是leaf-wise（xgboost对每一层所有节点进行无差别分裂，可能有些节点的增益非常小，对结果影响不大，但是xgboost也进行了分裂，开销太大了）（GBDT更容易过拟合，需要控制最大深度）
- 建树方式不同：Xgboost是exact，LGBM是hist（建树方式的原理说明；直方图可以直接相减计算）
- 并发不同（两个并发的原理）
- LGBM可以接受类别feature，类似于one-hot编码

## 3. 为什么xgboost使用泰勒展开？
使用泰勒展开是为了能够【自定义loss function】。
实际上，使用最小二乘法的损失函数进行直接推导和泰勒展开的推导结果相同。两者虽然结果相同，但是OLS的计算量太大了。在实际的代码过程中，任何损失函数只要二阶可导都可以【复用】泰勒展开，例如基于分类的对数损失函数。这样的话，【代码可以在分类和回归进行复用】。
## 4. Xgboost如何寻找最优特征？是有放回还是无放回？
Xgboost在训练的过程中给出各个特征的评分，从而表明每个特征对模型训练的重要性。
无放回抽样。Xgboost是梯度优化模型，如果一个样本连续重复抽出，则梯度来回踏步，不利于收敛。
## 5. gbdt原理
提升树是：计算每个样本的残差，对残差进行拟合得到回归树。
GBDT（梯度提升树）：使用loss function的偏导代替残差进行拟合。
## 6. xgboost源码看过吗
讲多线程计算的三个函数：
map -> reduce -> shift



# 深度学习

## 基本概念
### 1.卷积，池化
- 卷积（convolution）：输入矩阵和卷积核进行对应元素相乘并求和，所以一次卷积的结果的输出是一个数。然后对整个输入矩阵进行遍历，最终得到一个结果矩阵。*
  卷积有padding convolution
  ![2a86cd904f918682ef2ec08274815bb4.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p170)
  和no padding convolution
  ![391bed6faf069f3dbdfbb79f697c2238.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p169)
- 池化（pooling）：操作的过程与卷积类似，但是卷积是用来提取特征的，池化是用来减少卷积层提取的特征的个数的。可以理解为是为了增加特征的鲁棒性或者是降维。
  池化包括平均值池化
  ![42420738c54d377cbb98c0fdb6f84e3f.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p171)
  和最大值池化。
  ![306c88bec7d253cd3f029f5ff93fa0c4.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p175)
### 2.池化的作用
1.减少参数
2.提取了最显著的特征，有微小的平移不变性
3.增加感受眼（特征代表了原图中的内容）
池化是简单提取了特征，卷积进行了权重的计算
### 3.如何解决梯度爆炸
(1) 预训练+微调
Hinton在2006年发表论文，提出无监督逐层训练方法。基本思想是每次训练一层隐节点，训练时将上一层隐节点的输出作为输入，而本层隐节点的输出作为下一层隐节点的输入，次过程称为“预训练”（pre-training）；预训练完成后，再对整个网络进行“微调”（fine-tunning）。Hinton在训练深度信念网络（Deep Belief Networks中，使用了这个方法，在各层预训练完成后，再利用BP算法对整个网络进行训练。此思想相当于是先寻找局部最优，然后整合起来寻找全局最优，此方法有一定的好处，但是目前应用的不是很多了。
(2) 设置梯度剪切阈值
梯度剪切这个方案主要是针对梯度爆炸提出的，其思想是设置一个梯度剪切阈值，然后更新梯度的时候，如果梯度超过这个阈值，直接将梯度置为该值。
(3) 使用relu，leakrelu, elu等代替sigmoid激活函数
relu的表达式具体见上。如果激活函数的导数为1，则不存在梯度爆炸的问题，例如relu函数替代sigmoid和tanh。
(4) 设置batch normalization
Batchnorm全名是batch normalization，简称BN，即批规范化，通过规范化操作将输出信号x规范化保证网络的稳定性。
(5) 残差结构
(6) LSTM，长短期记忆网络（long-short term memory networks）


## CNN
输入层 -> 卷积层 -> 激活函数 -> 池化层 -> 全连接层
### 基本思想
- 局部感知(local field)
- 权值共享(Shared Weights)
- 下采样(subsampling)
  获得了某种程度的位移、尺度、形变不变性，提高了运算速度和精度。
### CNN与RNN的区别
![faf4edb64a43ce96ee5d024d40332e6d.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p177)
相同点：
- 前向计算产生结果，后向计算进行模型更新

不同点：
- CNN进行空间扩展，神经元和特征卷积；RNN进行时间扩展，神经元与多个时间输出计算
- RNN可以描述时间上连续状态的输出，有记忆功能；CNN用于静态输出
- CNN高级结构可以达到100+深度；RNN的深度有限

例如：
![d6009686179c653b7983b65649d82e68.png](evernotecid://F7EB078F-95A5-4DA9-8982-295B0B6446F0/appyinxiangcom/21940830/ENResource/p178)
one to one：表示的是CNN网络的场景，从固定的输入到固定的输出
one to many：RNN的场景，序列输出，有点像看图说话，例如固定了输入的图片，然后输出一段序列描述这个图的意义
many to one：RNN的场景，序列输入，比如我们做语义情感分析，输入一串不定长度的话，返回情绪
many to many：RNN的场景，常见的sequence to sequence，比如之前的一个文章到的，通过周杰伦的歌词数据，模仿写出一首周杰伦风格的歌词，这种场景的输入和输出的长度都是不定的。


### RNN、CNN的流程，区别
### 图像的预处理方式，特征，扩增，增强
### SSD的理论

### droupout
droupout本质是一个Bagging


### 解释RESNET
### 解释BN


### BP神经网络推导一遍
### 画lstm原理图
### 手推lstm公式

# 其他

### 如何把类别型数据转为数值型
### 数据缺失值处理
### 如何采集样本，样本类别不均衡对模型有什么影响
### 怎么样增加模型的泛化能力（并不是指的过拟合那种）

### SVM SMO原理？
### SVM目标函数，为什么转为对偶
-- ### SVM是现行可分的，在平面投影之后，还是现行可分的吗？
一定不，SVM是凸包的投影中垂线

### 泛化误差
### 手推维特比算法（华为）


### 介绍下聚类及原理
### 什么是Kmeans，与EM怎么联系
解释Kmeans如何体现EM理论
-### EM的收敛
### EM算法

### kd tree
### FFM的优化
### mapreduce思想

### 高斯混合模型GMM
### LR的多项式
LR的多项式就是用核函数，运用w是x的线性组合，引入内积、引入核函数就出来了
其他和SVM一样

### F1 score
### 精度
### p和r在不同情况下的模型优化
### 讲一下AUC
#### AUC的面积越大，准确度和精确度是不是越高？
#### 二分类随机分类，AUC值？

# 书籍推荐

## 《百面机器学习》

请回看本文第一句。

## 《深度学习500问》

好像还有个机器学习100问？500问？

## 《统计学习方法》

第二版加了NLP的东西。BTW，李航老师真的好温柔啊哈哈哈哈

## 《西瓜书》

一本你现在可能看不完看不懂看到放弃的书，不久之后，你会发现，你遇到的所有问题西瓜书都提醒你了。

## 常见深度网络相关论文

我的意思是，如果你有时间。

# 写在最后

在大厂的不全是大佬，也有混混。大厂可以成为你学习工作的目标，但我希望这不是你的唯一目标。

写这些是因为，大二时在网上下载了一份算法阿里星上传的简历，个人信息都被抹去了，默默保留了很久。没承想，实习期间竟然见到了本人还加了联系方式。

但愿这个文档有用。



