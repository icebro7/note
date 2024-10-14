# 决策树



| 参数                  | 含义                                                         |
| --------------------- | ------------------------------------------------------------ |
| criterion             | 不纯度的衡量指标，有基尼系数和信息熵两种选择                 |
| max_depth             | 树的最大深度，超过最大深度的树枝都会被剪掉                   |
| min_samples_leaf      | 一个节点在分枝后的每个子节点都必须包含至少min_samples_leaf个训练样 本，否则分枝就不会发生 |
| min_samples_split     | 一个节点必须要包含至少min_samples_split个训练样本，这个节点才允许被分 枝，否则分枝就不会发生 |
| max_features          | max_features限制分枝时考虑的特征个数，超过限制个数的特征都会被舍弃， 默认值为总特征个数开平方取整 |
| min_impurity_decrease | 限制信息增益的大小，信息增益小于设定数值的分枝不会发生       |



```python
# 模块sklearn.tree

# tree.DecisionTreeClassifier  分类树
# tree.DecisionTreeRegressor   回归树
# tree.export_graphviz		  决策树导出成DOT格式
# tree.ExtraTreeClassifier	   高随机版本的分类树
# tree.ExtraTreeRegressor	   高随机版本的回归树

```

![屏幕截图 2022-04-15 150423](C:\Users\98680\Desktop\学习笔记\sklearn\printscreen\屏幕截图 2022-04-15 150423.png)



#  一、分类树

## 				1、criterion

```python
# entropy，信息熵（Entropy）

# gini，基尼系数（Gini Impurity）

# 数据维度很大，噪音很大时使用基尼系数
# 当决策树的拟合程度不够的时候，使用信息熵

# 维度低，数据比较清晰的时候，信息熵和基尼系数没区别

```



```python
# 建立一颗树

# 1.导入需要的算法库和模块
from sklearn import tree
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split

# 2.查看数据
wine = load_wine()

wine.data.shape

wine.target

#如果wine是一张表，应该长这样：
import pandas as pd
pd.concat([pd.DataFrame(wine.data),pd.DataFrame(wine.target)],axis=1)
```

![image-20220416181030095](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220416181030095.png)



```python
# 包含所有特征名称的，长度为特征名称个数的列表。

wine.feature_names
```

![image-20220416181249949](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220416181249949.png)

```python
# 标签名字

wine.target_names
```

![image-20220416181707794](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220416181707794.png)

 

```python
# 3.分训练集和测试集

Xtrain, Xtest, Ytrain, Ytest = train_test_split(wine.data,wine.target,test_size=0.3) #test_size为分的比例

Xtrain.shape
Xtest.shape

# 4.建立模型

clf = tree.DecisionTreeClassifier(criterion="entropy"#采用的为信息熵
                                  ,random_state=30
								,splitter="random"
                               # 增加随机度，防止过拟合)

clf = clf.fit(Xtrain, Ytrain)
score = clf.score(Xtest, Ytest) #返回预测的准确度

score

# 5.画出一颗树
feature_name = ['酒精','苹果酸','.','灰的碱性','镁','总酚','类黄酮','非黄烷类酚类','花青素','颜色强度','色调','od280/od315稀释葡萄酒','脯氨酸']

import graphviz
dot_data = tree.export_graphviz(clf
                                ,out_file=None
                                ,feature_names= feature_name
                                ,class_names=["琴","雪","贝"]
                                ,filled=True
                                ,rounded=True
                               )
graph = graphviz.Source(dot_data)
graph
```

![image-20220416182153915](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220416182153915.png)



```python
# 特征重要性

clf.feature_importances_

[*zip(feature_name,clf.feature_importances_)]
```

![image-20220416213513837](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220416213513837.png)



## 2、剪枝

```python
# max_depth
	# 限制树的最大深度，超过的全部剪掉
    
# min_samples_leaf
	# 一个节点在分枝后的每个子节点都必须包含至min_samples_leaf个训练样本，否则分枝就不会发生，或者朝向方向转变

# min_samples_split
	# 一个节点必须要包含至少min_samples_split个训练样本，这个节点才允许被分枝
    
# max_features
	# 限制分枝时考虑的特征个数，超过限制个数的特征都会被舍弃
   
# min_impurity_decrease
	# 限制信息增益的大小，信息增益小于设定数值的分枝不会发生
    
# class_weight
	# 调整目标权重
 
# min_weight_fraction_leaf
	# 基于权重的剪枝参数

import graphviz

feature_name = ['酒精','苹果酸','灰','灰的碱性','镁','总酚','类黄酮','非黄烷类酚类','花青素','颜色强度','色调','od280/od315稀释葡萄酒','脯氨酸']    
    
    
clf = tree.DecisionTreeClassifier(criterion="entropy"
                                  ,random_state=30
                                #  ,splitter="random"
                                  ,max_depth=3
                               #  ,min_samples_leaf=20
                                  ,min_samples_split=42
                                 )

clf = clf.fit(Xtrain, Ytrain)

dot_data = tree.export_graphviz(clf
                                ,feature_names= feature_name
                                ,class_names=["琴酒","雪莉","贝尔摩德"]
                                ,filled=True
                                ,rounded=True
                               )  
graph = graphviz.Source(dot_data)
graph

clf.score(Xtrain,Ytrain)
clf.score(Xtest,Ytest)
```



## 3、学习曲线

```python
import matplotlib.pyplot as plt

test = []
for i in range(10):
    
    clf = tree.DecisionTreeClassifier(max_depth=i+1
                                      ,criterion="entropy"
                                      ,random_state=30
                                      ,splitter="random"
                                     )
    clf = clf.fit(Xtrain, Ytrain)
    score = clf.score(Xtest, Ytest)
    test.append(score)
plt.plot(range(1,11),test,color="red",label="max_depth")
plt.legend()
plt.show()
```

![image-20220416224454758](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220416224454758.png)



## 4、属性和接口

```python
# 八个参数：Criterion，两个随机性相关的参数（random_state，splitter），
#四个剪枝参数（max_depth, ，min_sample_leaf，min_samples_split,max_feature，min_impurity_decrease）

# 一个属性：feature_importances_
# 四个接口：fit，score，apply，predict

```



# 二、回归树

## 1、criterion

```python
# mse，误均方差
# friedman_mse， 费尔德曼均方误差，由mse改进的方差
# mae，绝对平均误差
```



## 2、代码

```python
# 建立一颗树

from sklearn.datasets import load_boston
from sklearn.model_selection import cross_val_score
from sklearn.tree import DecisionTreeRegressor
from sklearn import tree
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split
import matplotlib.pyplot as plt
import numpy as np

regressor = tree.DecisionTreeRegressor(random_state=0)

boston = load_boston() # 引入数据

cross_val_score(regressor,boston.data,boston.target,cv=10).mean() # 交叉验证

boston = load_boston()
regressor = DecisionTreeRegressor(random_state=0)
cross_val_score(regressor,boston.data,boston.target,cv=10
               ,scoring="neg_mean_squared_error")
	# r^2
    
rng = np.random.RandomState(1) #随机树种子

x = np.sort(5 * rng.rand(80,1),axis = 0) #排序，横坐标数据，生成0~5之间随机的x取值

y = np.sin(x).ravel() # 生成正弦曲线，ravel降维

plt.figure()
plt.scatter(x,y,s=20,edgecolors="black",c="darkorange",label="data") # 画图
```

![image-20220418082319923](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220418082319923.png)



```python
y[::5] += 3 * (0.5 - rng.rand(16)) # 添加噪声,3用于扩大数值，0.5相当于添加-0.5到0.5的区间

regr_1 = DecisionTreeRegressor(max_depth=2)
regr_2 = DecisionTreeRegressor(max_depth=5)
regr_1.fit(x,y)

regr_2.fit(x,y)

x_text = np.arange(0.0,5.0,0.01)[:,np.newaxis] # arange生成有效序列，newaxis切片增维

y_1 = regr_1.predict(x_text)
y_2 = regr_2.predict(x_text)

# 画图
plt.figure()
plt.scatter(x,y,s=20,edgecolor = "black",c="darkorange",label="data")
plt.plot(x_text,y_1,color="red",label="max_depth=2",linewidth=2)
plt.plot(x_text,y_2,color="green",label="max_depth=5",linewidth=2)
plt.xlabel("data")
plt.ylabel("target")
plt.title("DTR")
plt.legend()
plt.show() 
```

![image-20220418082519326](C:\Users\98680\AppData\Roaming\Typora\typora-user-images\image-20220418082519326.png)



## 3、交叉验证

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(clf,x,y,cv=10)
```



## 4、学习曲线

```python
label = "RandomForest"
for model in [RandomForestClassifier(n_estimators=25),DecisionTreeClassifier()]:
   score = cross_val_score(model,wine.data,wine.target,cv=10)
   print("{}:".format(label)),print(score.mean())
   plt.plot(range(1,11),score,label = label)
   plt.legend()
   label = "DecisionTree"
```





















## 5、泰坦尼克号案例

```python
import pandas as pd
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.model_selection import GridSearchCV
from sklearn.model_selection import cross_val_score
import matplotlib.pyplot as plt

#导入data，并设置第一列为index
data = pd.read_csv("./temp/cai/day08_data.csv",index_col = 0)

#删除缺失值过多的列
data.drop(["Cabin","Name","Ticket"],inplace=True,axis=1)

#处理缺失值，对缺失值较多的列进行填补
data["Age"] = data["Age"].fillna(data["Age"].mean())
data = data.dropna()

#将二分类变量转换为数值型变量
data["Sex"] = (data["Sex"]== "male").astype("int")

#将三分类变量转换为数值型变量
labels = data["Embarked"].unique().tolist()
data["Embarked"] = data["Embarked"].apply(lambda x: labels.index(x))

# 特征标签分离
x = data.iloc[:,data.columns != "Survived"]
y = data.iloc[:,data.columns == "Survived"]

from sklearn.model_selection import train_test_split
xtrain, xtest, ytrain, ytest = train_test_split(x,y,test_size=0.3)

#修正测试集和训练集的索引
for i in [xtrain, xtest, ytrain, ytest]:
    i.index = range(i.shape[0])
    
# 训练打分
clf = DecisionTreeClassifier(random_state=25)
clf = clf.fit(xtrain,ytrain)
score = clf.score(xtest,ytest)
score   #0.7715355805243446

# 学习曲线
tr = []
te = []
for i in range(10): # 做十次
    clf = DecisionTreeClassifier(random_state=25
                                 ,max_depth=i+1
                                 ,criterion="entropy"
                                )
    clf = clf.fit(xtrain, ytrain)
    score_tr = clf.score(xtrain,ytrain)
    score_te = cross_val_score(clf,x,y,cv=10).mean()
    tr.append(score_tr)
    te.append(score_te)
print(max(te))
plt.plot(range(1,11),tr,color="red",label="train")
plt.plot(range(1,11),te,color="blue",label="test")
plt.xticks(range(1,11))
plt.legend()
plt.show()


# 网格搜索
import numpy as np
gini_thresholds = np.linspace(0,0.5,20)

parameters = {'splitter':('best','random')
              ,'criterion':("gini","entropy")
              ,"max_depth":[*range(1,10)]
              ,'min_samples_leaf':[*range(1,50,5)]
              ,'min_impurity_decrease':[*np.linspace(0,0.5,20)]
             }

clf = DecisionTreeClassifier(random_state=25)
GS = GridSearchCV(clf, parameters, cv=10)
GS.fit(xtrain,ytrain)

# 返回组合
GridSearchCV(cv=10, estimator=DecisionTreeClassifier(random_state=25),
             param_grid={'criterion': ('gini', 'entropy'),
                         'max_depth': [1, 2, 3, 4, 5, 6, 7, 8, 9],
                         'min_impurity_decrease': [0.0, 0.02631578947368421,
                                                   0.05263157894736842,
                                                   0.07894736842105263,
                                                   0.10526315789473684,
                                                   0.13157894736842105,
                                                   0.15789473684210525,
                                                   0.18421052631578946,
                                                   0.21052631578947367,
                                                   0.23684210526315788,
                                                   0.2631578947368421,
                                                   0.2894736842105263,
                                                   0.3157894736842105,
                                                   0.3421052631578947,
                                                   0.3684210526315789,
                                                   0.39473684210526316,
                                                   0.42105263157894735,
                                                   0.4473684210526315,
                                                   0.47368421052631576, 0.5],
                         'min_samples_leaf': [1, 6, 11, 16, 21, 26, 31, 36, 41,
                                              46],
                         'splitter': ('best', 'random')})


#从输入的参数中取出最佳组合
GS.best_params_
# 
{'criterion': 'gini',
 'max_depth': 3,
 'min_impurity_decrease': 0.0,
 'min_samples_leaf': 6,
 'splitter': 'best'}

#最佳精确性
GS.best_score_
# 0.8408090117767536



```



# 三、随机森林



## 1、随机森林分类器

### 	(1) n_estimators

```python
from sklearn.ensemble import RandomForestClassifier

rfc = RandomForestClassifier(n_estimators=25) # 创建25棵树
```

### (2)estimators_

```python
# 在训练模型之后，查看树的模型

rfc.estimators_

# 查看每颗树的信息

for i in range(len(rfc.estimators_))：
	print(rfc.estimators_[i])
```



### (3) random_state



### (4) bootstrap



### (5)oob_score

```python
#无需划分训练集和测试集
rfc = RandomForestClassifier(n_estimators=25,oob_score=True)
rfc = rfc.fit(wine.data,wine.target)

#重要属性oob_score_
rfc.oob_score_

```



## 2、随机森林回归器

