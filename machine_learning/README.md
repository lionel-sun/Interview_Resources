# 机器学习

目标：项目涉及内容，基本算法

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `基本问题`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `集成学习boosting，stacking，bagging`

bagging 减少方差，多个预测结果取平均

boosting 减少偏差，的是通过算法集合将弱学习器转换为强学习器。方法训练基分类器时采用串行的方式，各个基分类器之间有依赖。
其基本思想是根据当前模型损失函数的负梯度信息来训练新加入的弱分类器，然后将训练好的弱分类器以累加的形式结合到现有模型中。

stacking 提升预测结果，通过一个元分类器或者元回归器来整合多个分类模型或回归模型的集成学习技术

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `GBDT`

GBDT是基于决策树的boosting集成学习算法，决策树分为回归树（预测值）和分类树（分类）。GBDT中的决策树是回归树。使用多个回归树，在当前决策树误差基础上，
构建下一个决策树去降低这个误差。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `XGboost和LightGBM`

XGB是建立在GBDT基础上的算法模型：
XGBoost将树模型的复杂度加入到正则项中，从而避免过拟合，泛化性能好；损失函数是用泰勒展开式展开的，用到了一阶导和二阶导，可以加快优化速度；
在寻找最佳分割点时，采用近似贪心算法，用来加速计算；不仅支持CART作为基分类器，还支持线性分类器，在使用线性分类器的时候可以使用L1，L2正则化；
支持并行计算，XGBoost的并行是基于特征计算的并行，将特征列排序后以block的形式存储在内存中，在后面的迭代中重复使用这个结构。
在进行节点分裂时，计算每个特征的增益，选择增益最大的特征作为分割节点，各个特征的增益计算可以使用多线程并行。

优点：速度快、效果好、能处理大规模数据、支持自定义损失函数等。
缺点：算法参数过多，调参复杂，不适合处理超高维特征数据。


LightGBM是2017年经微软推出，XGBoost的升级版：在大规模数据集上运行效率更高，解决了训练海量数据的问题。比xgb训练速度快=>1/10，内存消耗低=>1/6。
xgb不支持分类特征，需要onehot，lgbm支持。针对xgb的优化：Histogram算法，直方图算法 => 减少候选分裂点数量。
GOSS算法，基于梯度的单边采样算法 => 减少样本的数量。EFB算法，互斥特征捆绑算法 => 减少特征的数量。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `catboost，NGboost`

CatBoost 设计了一种算法验证改进，避免了过拟合。因此处理分类数据比LightGBM 和XGBoost 强。准确性高，同时比xgb速度快。支持gpu，可以处理缺失值。

自然梯度提升（NGBoost / Natural Gradient Boosting）一种用于概率预测的自然梯度增强算法。
预估机器学习模型在预测中的不确定性对于生产环境非常重要。这是因为我们不仅希望模型能做出准确的预测，同时还希望在预测时能够估计不确定性。
概率预测是一种量化这种不确定性的自然方法，这种模型会输出在整个结果空间上的完整概率分布。选择概率最大的方向下降。结果更优，但是计算耗时更长。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `GBDT等树模型为啥不适合处理高纬度稀疏特征`

树模型可以直接处理分类特征，不需要做onehot。onehot后的特征是高度稀疏的，如果刚好某一个值的结果一致可能导致过拟合。
树模型的正则惩罚项是针对深度和叶子节点数，对这种情况下的惩罚项极小。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `DIN和DSIN模型介绍`

一般做法是MLP全连接的DNN网络。

DIN引入attention机制，捕获候选商品和用户浏览过的商品之间的关系（兴趣），每个历史记录增加了一个权重就是和候选产品的关系。加权后历史行为输入DNN。

DSIN将行为序列划分为session，30分钟作为节点切割一个session。每个会话可以表示兴趣的演变，使用LSTM或者GRU吧。

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `调整参数`


## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `项目相关`

项目收获？ 为啥用某个技术？相关技术原理，和别的比优势？使用什么参数，怎么调参？涉及简单的公式，最后成绩如何？如何优化？

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `天猫复购`

这个项目背景是通过预测潜在忠诚客户，赠与优惠卷提高复购率。先将用户信息和商家信息关联起来（包括测试集合训练集）。然后分别通过历史日志信息构造新得特征值。
构造了10个基于用户的统计特征（包括交互商家个数，产品个数，品牌个数，不同类型操作个数）构造10个左右商家特征，还有基于用户和商家共同特征8个。
还有用户购买点击比，不同用户不同商家的购买点击比等。使用基本参数加GridSearchCV调参，最后就得到0.68的成绩在长期赛里面top80。基本上是非NN模型的最高成绩。

NN模型创建用户行为序列，长度取500个（超过的去除，不足的补全）。使用了DEEPCTR的DIN和DSIN模型。


- [地址](https://github.com/lionel-sun/Tmall_Repeat_Buyers)

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `BBC食谱搜索引擎`

使用selenium爬取数据。nltk进行词处理，取词干还有去除停用词，然后生成倒排索引，根据查询词的使用倒排索引出相关文档，然后使用cos计算相似度。
根据相似度进行排序。缺点是只考虑词频，并没有考虑词义，也体现不了上下文含义，使用word2vec进行词向量获取搜索会更准确。Skip-gram 和 CBOW 模型
如果是用一个词语作为输入，来预测它周围的上下文，那这个模型叫做『Skip-gram 模型』
而如果是拿一个词语的上下文作为输入，来预测这个词语本身，则是 『CBOW 模型』

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `netflix电影评分预测`

导入txt文件进行预处理，合并所有txt，然后剔除probe文件中的部分数据后的数据集做训练集，probe做测试集。
加载surprise包，其中的baseline，funksvd，biassvd，svd++算法。后面又增加了使用pyspark的包进行离线计算，（colab直接pip安装spark）
spark的ml包基于dataframe（mllib基于rdd的，3.0版本后会弃用所以没有使用），推荐使用。使用createDataFrame加载之前处理好的数据，
然后使用ml的als交替最小二分解矩阵进行评分预测及推荐。最小二乘法不会过拟合

- [地址](https://github.com/lionel-sun/Netflix)

## 引用资源
- [ML基础总结](https://github.com/lionel-sun/RS_Practice/blob/master/AI算法岗位面试资料整理/README.md)

- [RS总结](https://github.com/lionel-sun/RS_Practice/blob/master/README.md)

- [别人总结的一些ML问题](https://github.com/wangyuGithub01/Machine_Learning_Resources)

- [2021届校招算法岗知识点总结](https://zhuanlan.zhihu.com/p/107911095)

- [一篇文章搞定GBDT、Xgboost和LightGBM的面试](https://zhuanlan.zhihu.com/p/148050748)

- [为什么xgboost不适合高维稀疏特征](https://www.zhihu.com/question/267934807)

- [GBDT 如何处理高维度稀疏特征？](https://www.zhihu.com/question/55925445)

- [Sklearn中Pipeline的使用](https://www.jianshu.com/p/9c2c8c8ef42d)

- [Python机器学习基础教程》六、算法链与管道](https://zhuanlan.zhihu.com/p/48247268)