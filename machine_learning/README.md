# 机器学习

目标：项目涉及内容，基本算法

## ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `基本问题`

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `集成学习boosting，stacking，bagging`

bagging 减少方差，多个预测结果取平均

boosting 减少偏差，的是通过算法集合将弱学习器转换为强学习器。方法训练基分类器时采用串行的方式，各个基分类器之间有依赖。
其基本思想是根据当前模型损失函数的负梯度信息来训练新加入的弱分类器，然后将训练好的弱分类器以累加的形式结合到现有模型中。

stacking 提升预测结果，通过一个元分类器或者元回归器来整合多个分类模型或回归模型的集成学习技术

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `GBDT`

在每一轮迭代中，首先计算出当前模型在所有样本上的负梯度，然后以该值为目标训练一个新的弱分类器进行拟合并计算出该弱分类器的权重，最终实现对模型的更新。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `XGboost和LightGBM区别`

XGB高效地实现了GBDT算法并进行了算法和工程上的许多改进。原始的GBDT算法基于经验损失函数的负梯度来构造新的决策树，只是在决策树构建完成后再进行剪枝。
而XGBoost在决策树构建阶段就加入了正则项。GBDT是机器学习算法，XGBoost是该算法的工程实现。

LGBM是XGBoost的改进版，添加了很多新的方法来改进模型，并行方案、基于梯度的单边检测、排他性特征捆绑等。

### ![#1589F0](https://placehold.it/15/1589F0/000000?text=+) `GBDT，XGboost，LightGBM，Catboost, NGboost`

自然梯度提升（NGBoost / Natural Gradient Boosting）是一种算法其以通用的方式将概率预测能力引入到了梯度提升中。
预测式不确定性估计在医疗和天气预测等很多应用中都至关重要。
概率预测是一种量化这种不确定性的自然方法，这种模型会输出在整个结果空间上的完整概率分布。选择概率最大的方向下降。
结果更优，但是计算耗时更长。

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