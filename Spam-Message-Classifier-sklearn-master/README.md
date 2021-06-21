# Spam-Message-Classifier-sklearn
Classification of spam messages with SVM-linear,SVM-rbf and Naive Bayes by scikit-learn
## Intro to spam message
The task is to classify spam messages which are composed of Chinese characters, English, Numbers and some special characters. 
Normal messages should be identified as 0, and spam messages as 1.

Snapshoot of training data : `RawData/junk_message.txt`

```
0	但是没有首都的在CBD一家好吃
0	今天给大家介绍一款Bookbook电脑包
1	亲爱的家长朋友你们好，我是张老师:新学期现在开始报名啦！本学期对老生家长的义务宣传有个回馈活动，老生带一个新生报名，赠送老生二百元兴趣
0	浙江宁波市马路突然爆裂
1	各位亲们好，新世纪奉节店迎接三八节，贝因美奶粉全场x.x折，欢迎惠额。活动时间x月x日至x月x日。
```

## Prerequisite
* [Python 2.7.x](https://www.python.org/downloads/)
* [Scikit-learn](http://scikit-learn.org/stable/documentation.html#)
* [jieba](https://github.com/fxsjy/jieba)

## Usage
### [Blog详细说明](http://blog.csdn.net/github_36922345/article/details/53455401)


Fork the repository, then clone or download

```
$ git clone https://github.com/ZPdesu/Spam-Message-Classifier-sklearn
```

进入项目文件夹下，从RawData/junk_message.txt中加载原始数据，进行文本域和标签域分割，转存到对应json文件中

```
$ cd Spam-Message-Classifier-sklearn
$ python load_data.py
```

对短信文本进行筛选和处理转换后进行分词，将生成的词向量稀疏矩阵存入Data/word_vector.mtx，词向量对应的词表存入Data/vector_type.json，
标签信息存入`Data/train_label.json

```
$ python word_vector.py
```

将详细的信息，如词表，词向量对应的词表索引，标签信息等存入MongoDB数据库

```
$ python Mongo_Configure.py
```

SVM目录下包含了封装了模型训练方法的类SVM_Trainer，预测和检验输出方法的类SVM_Predictor，和最终运行和评价模型的类SVM_Evaluator。
持久化后的训练模型存储在model目录下对应的pkl文件中，fig目录中存储着寻找最优参数时的图像，预测的分类结果存储在result目录下,运行SVM_Trainer.py或者SVM_Evaluator.py将进行数据集分割，数据降维（PCA or Random projections），训练，交差验证验证及预测等功能。

### SVM
```
.
├── SVM_Trainer.py
├── SVM_Trainer.pyc
├── SVM_Predictor.py
├── SVM_Predictor.pyc
├── SVM_Evaluator.py
├── __init__.py
├──fig 
│  └──param_effect.png
├──result 
│  └──predict_label.txt
└── model
    ├── SVM_linear.pkl
    ├── SVM_rbf.pkl
    └── Terminal.pkl
```

**In**

```
$ python SVM_Trainer.py
```

**Out**

best parameters, classification report, N-fold cross validation accuracy

```
The best parameters are {'C': 10000.0} with a score of 0.97
             precision    recall  f1-score   support

          0       1.00      1.00      1.00      8128
          1       1.00      1.00      1.00       872

avg / total       1.00      1.00      1.00      9000

[ 0.89272305  0.88408158  0.90056112  0.89901823  0.89448291]
Accuracy: 0.89 (+/- 0.01)

```
![Image](https://github.com/ZPdesu/Spam-Message-Classifier-sklearn/blob/master/SVM/fig/param_effect.png)

**In**

```
$ python SVM_Evaluator.py
```

**Out**

classification report, confusion matrix

```
             precision    recall  f1-score   support

          0       0.98      0.98      0.98       906
          1       0.80      0.82      0.81        94

avg / total       0.96      0.96      0.96      1000

[[887  19]
 [ 17  77]]

```

## Structure

```
├── SVM
│   ├── SVM_Trainer.py
│   ├── SVM_Trainer.pyc
│   ├── SVM_Predictor.py
│   ├── SVM_Predictor.pyc
│   ├── SVM_Evaluator.py
│   ├── __init__.py
│   ├──fig 
│   │  └──param_effect.png
│   ├──result 
│   │  └──predict_label.txt
│   └── model
│       ├── SVM_linear.pkl
│       ├── SVM_rbf.pkl
│       └── Terminal.pkl
├── Naive_Bayes
│   ├── Bayes_Trainer.py
│   ├── Bayes_Trainer.pyc
│   ├── Bayes_Predictor.py
│   ├── Bayes_Predictor.pyc
│   ├── Bayes_Evaluator.py
│   ├── __init__.py
│   ├──result 
│   │  └──predict_label.txt
│   └── output
│       ├── Bayes.pkl
│       └── Terminal.pkl
├── Mongo_Configure.py
├── load_tata.py
├── load_tata.pyc
├── word_vector.py
├── word_vector.pyc
├── preprocessing.py
├── preprocessing.pyc
├── test.py
├── RawData
│   ├── junk_message.txt
│   ├── message.txt
│   ├── train_label.json
│   └── train_content.json
└── Data
    ├── train_label.json
    ├── vector_type.json
    └── word_vector.mtx
```
