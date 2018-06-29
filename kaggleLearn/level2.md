# 丢失数据处理
## 统计各列是空的元素个数
data.isnull().sum()
## 丢掉带有丢失数据的列
### How
#### 一般
>data_without_missing_values = original_data.dropna(axis=1)
#### 扩展
>cols_with_missing = [col for col in original_data.columns if original_data[col].isnull().any()]
redued_original_data = original_data.drop(cols_with_missing, axis=1)
reduced_test_data = test_data.drop(cols_with_missing, axis=1)
### When
当一列中有大量数据丢失的时候
### 特点
优点：简单
缺点：往往会丢失一些特性，模式，导致模型预测效果差
## Imputation
### Ｗhat
Rather than removing variables or observations with missing data, another approach
is to fill in or “impute” missing values. A variety of imputation approaches
can be used that range from extremely simple to rather complex. These methods
keep the full sample size, which can be advantageous for bias and precision; however,
they can yield different kinds of bias, as detailed in this section.
### How
#### 一般
>from sklearn.preprocessing import Imputer
my_imputer = Imputer()
data_with_imputed_values = my_imputer.fit_transform(original_data)
#### 扩展
//make copy to avoid changing original data (when Imputing)
>new_data = original_data.copy()

//make new columns indicating what will be imputed
>cols_with_missing = (col for col in new_data.columns if new_dat[c].isnull().any())
for col in cols_with_missing:
    new_data[col + '_was_missing'] = new_data[col].isnull()

//Imputation
>my_imputer = Imputer()
new_data = my_imputer.fit_transform(new_data)
### When
需要得到更加精确的模型

# 处理标签数据
##What
很多算法（例如随机森林），它只能作用在数值型的数据上。而实际的数据中必然有一些数据不是数值（比如：颜色属性：红，黄，蓝），所以需要将这些数据进行量化编码。常用的编码方式有，one-hot-encoding，即通过1/0来表示某一个属性值，但是这种编码方式有属性可取值的数量限制（如果取值范围比较大，则不适用这种方法）![one-hot例子](http://yuntu88.oss-cn-beijing.aliyuncs.com/fromlocal/mtimFxh.png "one-hot简例")
##How
>one_hot_encoded_training_predictors = pd.get_dummies(train_predictors)
one_hot_encoded_test_predictors = pd.get_dummies(test_predictors)
final_train, final_test = one_hot_encoded_training_predictors.align(one_hot_encoded_test_predictors, join='left', axis=1)

# XGBoost
## What
XGBoost is an implementation of the Gradient Boosted Decision Trees algorithm (scikit-learn has another version of this algorithm, but XGBoost has some technical advantages.) What is Gradient Boosted Decision Trees? We'll walk through a diagram.
![梯度上升决策树](http://yuntu88.oss-cn-beijing.aliyuncs.com/fromlocal/e7MIgXk.png "Gradient Boosted Decision Trees")
## How
### 关键点
To reach peak accuracy, XGBoost models require more knowledge and model tuning than techniques like Random Forest. 
#### 模型调优
* n_estimators：迭代次数  100-1000
* early_stopping_rounds：精度连续相同的次数 5（如果连续5次精度不发生变化，则停止迭代）与n_estimators连用
* learning_rate：学习速度 0.05
* n_jobs：并行训练模型的线程数

# partial dependence plots
## What
artial dependence plots show how each variable or predictor affects the model's predictions
# cross validation
## What
对数据集比较小的数据，如果只将数据一次划分为训练集，测试集，可能导致最终测试模型的质量不准确。所以需要对数据进行多次划分。
![交叉验证图解](http://image.tbyuantu.com/pics/2018-06-28/cdd47e39573728ef1c1834915a58be5e.png  "交叉验证图解")
# Data Leakage
## What
因为数据属性的相关关系，造成进行模型验证时，测得的模型准确性偏高。
### 为什么会偏高呢？
因为其使用了训练集以外的信息进行了模型构建
### Leaky Predictors
### Leaky Validation Strategies
## How
### 发现数据泄露
当你的模型预测精度特别高的时候，先不要高兴，你需要检查一下是否发生了数据泄露
### 最小化数据泄露


