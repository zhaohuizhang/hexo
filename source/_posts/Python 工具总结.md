title: Python 工具总结
tags:
  - 科学计算
  - 网络爬虫
  - 全栈工程师
  - 数据挖掘
  - 文本处理
  - 机器学习
id: 29
categories:
  - Python
date: 2015-04-21 02:08:57
---

## 一、Python网页爬虫工具集

自己动手获取数据

### 1，Scrapy

Scrapy, a fast high-level screen scraping and web crawling framework for Python.

《Scrapy轻松定制网络爬虫》

[官网](http://scrapy.org/)，[Github代码](https://github.com/scrapy/scrapy)

### 2，Beautiful Soup

You didn't write that awful page. You're just trying to get some data out of it. Beautiful Soup is here to help. Since 2004, it's been saving programmers hours or days of week on quick-turnaround screen scraping projects.

配合urllib使用，数据分析，清洗和获取的工具

[官网](http://www.crummy.com/software/BeautifulSoup/)

### 3，Python-Goose

Html Content/Article Extractor, web scrapping lib in Python

Goose最早是用Java写，现在用Scala重写，是一个Scala项目，Python-Goose用Python重写，依赖于Beautiful Soup。

[Github](https://github.com/grangier/python-goose)地址

## 二、Python文本处理工具集

网页爬取数据之后进行基本的文本处理，譬如对于英文来说要最基本的tokenize,对于中文，则需要中文分词，进一步的话，词性标注，句法分析，关键词提取，文本分类，情感分析。

### 1，NLTK-Natural Language Toolkit

NLTK is a leading platform for building Python programs to work with human language data. It provide easy-to-use interface to over 50 corpora and lexical resources such as WordNet, along with a suite of text processing libraries for classification, tokenization, stemming, tagging, parsing, and semantic, reasoning, and an active discussion forum.

《Natural Language Processing with Python》 《Python Text Processing with NLTK 2.0 Cookbook》

[官网](http://www.nltk.org/)，[Github](https://github.com/nltk/nltk)

### 2，Pattern

Pattern is a web mining module for the Python programming language. It has tools for data mining, natural language processing, machine learning, network analysis and canvas visualization.

Pattern由比利时安特卫普大学CLiPS实验室出品，客观的说，Pattern不仅仅是一套文本处理工具，它更是一套web数据挖掘工具，囊括了数据抓取模块（包括Google, Twitter, 维基百科的API，以及爬虫和HTML分析器），文本处理模块（词性标注，情感分析等），机器学习模块(VSM, 聚类，SVM）以及可视化模块等，可以说，Pattern的这一整套逻辑也是这篇文章的组织逻辑. 使用的是它的英文处理模块Pattern.en, 有很多很不错的文本处理功能，包括基础的tokenize, 词性标注，句子切分，语法检查，拼写纠错，情感分析，句法分析等，相当不错。

[官网](http://www.clips.ua.ac.be/pattern)

### 3，TextBlob: Simplified Text Processing

TextBlob is a Python library for processing textual data. It provides a simple API for diving into common natural language processing(NLP) tasks such as part-of-speech tagging, noun phrase extraction, sentiment analysis, classification, translation, and more.

TextBlob是一个很有意思的Python文本处理工具包，它其实是基于上面两个Python工具包NLKT和Pattern做了封装（TextBlob stands on the giant shoulders of NLTK and pattern, and plays nicely with both），同时提供了很多文本处理功能的接口，包括词性标注，名词短语提取，情感分析，文本分类，拼写检查等，甚至包括翻译和语言检测，不过这个是基于Google的API的，有调用次数限制。TextBlob相对比较年轻，有兴趣的同学可以关注。

[官网](http://textblob.readthedocs.org/en/dev/)，[Github](https://github.com/sloria/textblob)

### 4，MBSP for Python

MBSP is a text analysis system based on the TiMBL and MBT memory based learning applications developed at CLiPS and ILK. It provides tools for Tokenization and Sentence Splitting, Part of Speech Tagging, Chunking, Lemmatization, Relation Finding and Prepositional Phrase Attachment.

MBSP与Pattern同源，同出自比利时安特卫普大学CLiPS实验室，提供了Word Tokenization, 句子切分，词性标注，Chunking, Lemmatization，句法分析等基本的文本处理功能，感兴趣的同学可以关注。

[官网](http://www.clips.ua.ac.be/pages/MBSP)

### 5，Gensim: Topic modeling for humans

Gensim是一个相当专业的Python包，无论是代码还是文档。

[官网](http://radimrehurek.com/gensim/index.html),[Github](https://github.com/piskvorky/gensim)

### 6,langid.py: Stand-alone language identification system

语言检测是一个很有意思的话题，不过相对比较成熟，这方面的解决方案很多，也有很多不错的开源工具包，不过对于Python来说。langid目前支持97种语言的检测，提供了很多易用的功能，包括可以启动一个建议的server，通过json调用其API，可定制训练自己的语言检测模型等，可以说是“麻雀虽小，五脏俱全”。

[Github](https://github.com/saffsd/langid.py)

### 7,Jieba: 结巴中文分词

结巴分词，其功能包括支持三种分词模式（精确模式、全模式、搜索引擎模式），支持繁体分词，支持自定义词典等，是目前一个非常不错的Python中文分词解决方案。

[Github](https://github.com/fxsjy/jieba)

## 三、Python科学计算工具包

MATLAB = NumPy+SciPy+Matplotlib+iPython

《Python科学计算》

### 1,NumPy

NumPy几乎是一个无法回避的科学计算工具包，最常用的也许是它的N维数组对象，其他还包括一些成熟的函数库，用于整合C/C++和Fortran代码的工具包，线性代数、傅里叶变换和随机数生成函数等。NumPy提供了两种基本的对象：ndarray（N-dimensional array object）和 ufunc（universal function object）。ndarray是存储单一数据类型的多维数组，而ufunc则是能够对数组进行处理的函数。

[官网](http://www.numpy.org/)

### 2，SciPy: Scientific Computing Tools for Python

SciPy是一个开源的Python算法库和数学工具包，SciPy包含的模块有最优化、线性代数、积分、插值、特殊函数、快速傅里叶变换、信号处理和图像处理、常微分方程求解和其他科学与工程中常用的计算。其功能与软件MATLAB、Scilab和GNU Octave类似。 Numpy和Scipy常常结合着使用，Python大多数机器学习库都依赖于这两个模块。”—-引用自“Python机器学习库”

[官网](http://www.scipy.org/)

### 3，Matplotlib

matplotlib is a python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms.

[官网](http://matplotlib.org/)

### 4，iPython

“iPython 是一个Python 的交互式Shell，比默认的Python Shell 好用得多，功能也更强大。 她支持语法高亮、自动完成、代码调试、对象自省，支持 Bash Shell 命令，内置了许多很有用的功能和函式等，非常容易使用。 ” 启动iPython的时候用这个命令“ipython –pylab”，默认开启了matploblib的绘图交互，用起来很方便。

[官网](http://ipython.org/)

## 四、Python 机器学习、数据挖掘

### 1，scikit-learn: Machine Learning in Python

scikit-learn is an open source machine learning library for the python programming language. It features various classification, regression and clustering algorithms including support vector machines, logistic regression, naive Bayes, random forests, gradient boosting, k-meams and DBSCAN is designed to interoperate with the python numerical and scientific libraries NumPy and SciPy.

scikit-learn是一个基于NumPy，SciPy, Matplotlib的开源机器学习工具包，主要涵盖分类，回归和聚类算法，例如SVM，逻辑回归，朴素贝叶斯，随机森林，k-means等算法，代码和文档都非常不错，在许多Python项目中都有应用。

[官网](http://scikit-learn.org)

### 2，Pandas: Python Data Analysis Library

Pandas is a software library written for the Python programming language for data manipulation and analysis. In particular, it offers data structures and operations for manipulating numerical tables and time series.

Pandas也是基于NumPy和Matplotlib开发的，主要用于数据分析和数据可视化，它的数据结构DataFrame和R语言里的data.frame很像，特别是对于时间序列数据有自己的一套分析机制，非常不错。这里推荐一本书《Python for Data Analysis》，作者是Pandas的主力开发，依次介绍了iPython, NumPy, Pandas里的相关功能，数据可视化，数据清洗和加工，时间数据处理等，案例包括金融股票数据挖掘等，相当不错。

[官网](http://pandas.pydata.org/)

### 3,mlpy

mlpy is a Python module for Machine Learning built on top of NumPy/SciPy and the GNU Scientific Libraries.

mlpy provide a wide range of state-of-the-art machine learning methods for supervised and unsupervised problems and it is aimed at finding a reasonable compromise among modularity, maintainability, reproducibility, usability and efficiency. mlpy is multiplatform, it is Open Source, distributed under the GNU General Public License version 3.

[官网](http://mlpy.sourceforge.net/)

### 4，MDP：The Modular toolkit for Data Processing

Modular toolkit for Data Processing (MDP) is a Python data processing framework.
From the user’s perspective, MDP is a collection of supervised and unsupervised learning algorithms and other data processing units that can be combined into data processing sequences and more complex feed-forward network architectures.
From the scientific developer’s perspective, MDP is a modular framework, which can easily be expanded. The implementation of new algorithms is easy and intuitive. The new implemented units are then automatically integrated with the rest of the library.
The base of available algorithms is steadily increasing and includes signal processing methods (Principal Component Analysis, Independent Component Analysis, Slow Feature Analysis), manifold learning methods ([Hessian] Locally Linear Embedding), several classifiers, probabilistic methods (Factor Analysis, RBM), data pre-processing methods, and many others.

[官网](http://mdp-toolkit.sourceforge.net/)

### 5，PyBrain

PyBrain is a modular Machine Learning Library for Python. Its goal is to offer flexible, easy-to-use yet still powerful algorithms for Machine Learning Tasks and a variety of predefined environments to test and compare your algorithms.

PyBrain is short for Python-Based Reinforcement Learning, Artificial Intelligence and Neural Network Library. In fact, we came up with the name first and later reverse-engineered this quite descriptive “Backronym”.

PyBrain正如其名，包括神经网络、强化学习(及二者结合)、无监督学习、进化算法。因为目前的许多问题需要处理连续态和行为空间，必须使用函数逼近(如神经网络)以应对高维数据。PyBrain以神经网络为核心，所有的训练方法都以神经网络为一个实例。

[官网](http://www.pybrain.org/)

### 6，PyML-machine Learning in Python

“PyML是一个Python机器学习工具包，为各分类和回归方法提供灵活的架构。它主要提供特征选择、模型选择、组合分类器、分类评估等功能。”

[官网](http://pyml.sourceforge.net/)

### 7，Milk: Machine learning toolkit in Python

Its focus is on supervised classification with several classifiers available: SVMs (based on libsvm), k-NN, random forests, decision trees. It also performs feature selection. These classifiers can be combined in many ways to form different classification systems.

[官网](http://luispedro.org/software/milk)

### 8，PyMVPA：MulitVariate Pattern Analysis(MVPA) in Python

PyMVPA is a Python package intended to ease statistical learning analyses of large datasets. It offers an extensible framework with a high-level interface to a broad range of algorithms for classification, regression, feature selection, data import and export. It is designed to integrate well with related software packages, such as scikit-learn, and MDP. While it is not limited to the neuroimaging domain, it is eminently suited for such datasets. PyMVPA is free software and requires nothing but free-software to run.

[官网](http://www.pymvpa.org/)

### 9，Pyrallel-Parallel Data Analytics in Python

“Pyrallel(Parallel Data Analytics in Python)基于分布式计算模式的机器学习和半交互式的试验项目，可在小型集群上运行”

[Github](http://github.com/pydata/pyrallel)

### 10,Monte - gradient based learning in Python

“Monte (machine learning in pure Python)是一个纯Python机器学习库。它可以迅速构建神经网络、条件随机场、逻辑回归等模型，使用inline-C优化，极易使用和扩展。”

[官网](http://montepython.sourceforge.net)

### 11，Theano

“Theano 是一个 Python 库，用来定义、优化和模拟数学表达式计算，用于高效的解决多维数组的计算问题。Theano的特点：紧密集成Numpy；高效的数据密集型GPU计算；高效的符号微分运算；高速和稳定的优化；动态生成c代码；广泛的单元测试和自我验证。自2007年以来，Theano已被广泛应用于科学运算。theano使得构建深度学习模型更加容易，可以快速实现多种模型。

[官网](http://deeplearning.net/software/theano/)，[Github](https://github.com/Theano/Theano)

### 12,Pylearn2

“Pylearn2建立在theano上，部分依赖scikit-learn上，目前Pylearn2正处于开发中，将可以处理向量、图像、视频等数据，提供MLP、RBM、SDA等深度学习模型。

[官网](http://deeplearning.net/software/pylearn2/)