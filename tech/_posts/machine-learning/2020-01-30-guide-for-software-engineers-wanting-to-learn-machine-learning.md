---
title: An Engineer's trek into Machine Learning
excerpt: "A guide for software engineers wanting to learn data science, machine learning,  and deep learning, with intro to the basics and list of online resources."
image: "https://4.bp.blogspot.com/-vTTgpLbLNIY/XkYUrf4G00I/AAAAAAAATDU/VfLiDGPsFzI12RhX_kTV4JiTEY9yopqiQCKgBGAsYHg/s1600/AC_Day11_367.JPG"
image_thumbnail: "https://4.bp.blogspot.com/-vTTgpLbLNIY/XkYUrf4G00I/AAAAAAAATDU/VfLiDGPsFzI12RhX_kTV4JiTEY9yopqiQCKgBGAsYHg/s320/AC_Day11_367.JPG"
tags:
  - "machine-learning"
  - "data-science"
  - "deep-learning"
  - "artificial-intelligence"
---

![](https://4.bp.blogspot.com/-vTTgpLbLNIY/XkYUrf4G00I/AAAAAAAATDU/VfLiDGPsFzI12RhX_kTV4JiTEY9yopqiQCKgBGAsYHg/s1600/AC_Day11_367.JPG){: width="100%" class="framedimg"}

You are a Software Engineer. You notice Artificial Intelligence, Machine Learning, Deep Learning, Data Science buzzwords all around. You wonder what these phrases mean, whether all this is for real and useful or is yet another hype and passing fad.

You want to figure out how it is changing or will change the computer/IT industry, and why you should care, if at all. You google about it, you read various articles, blogs, and tutorials. You get some idea but are also overwhelmed by the enormous wealth of math, tools, frameworks you discover.

You wish if someone could give an overview, say, a map and compass suitable for an engineer, to help you embark on the journey of mastering it all. This blog post is for you.

Like you, I am a software engineer and have been through that journey. My experience is relevant, and you can use it for charting out your own course. Apart from programming, I also love to trek in the Himalayas. I see interesting parallels in my ML journey and high altitude treks. Through those parallels, I will explain how you can plan and go on an ML expedition.

## A Trekking Expedition

A trek has the following four stages.

**Irresistible Allure:** I am drawn to a trek by hearing or reading about it. The peaks, trails, valley, and terrain. People gushing that it is a beautiful and challenging trek. It is an allure difficult to resist. But before I invest time, energy and resources, I ask: is the trail worth it? What is the experience like? Why should I go on it? I read people's blogs, see photos, spend time on Google Earth. In the end, I either say "Naah!" or I am even more hooked into doing it.

**Study the Terrain:** That is when the real work starts. Every trek has its challenges. So I study the terrain on Google Maps and Google Earth. I spend time looking at elevation maps, reading blogs, understanding points of no returns, and possible escape routes. I want to know key geographical features, difficulties, and dangers.

**Train Hard:** Once it is clear what I am up to, I plan and prepare. I start training hard, make a detailed plan, and regularly assess my progress.

**Check your Gears:** Finally, I stock the supplies and check my gears, especially the map and compass. I ensure that the map is ingrained in my mind, and I know enough about the vicinity to have an intuitive sense of direction.

And then the adventure begins!

Let's examine these four stages of the expedition to learn Machine Learning.

## Irresistible Allure

Like a vast number of software engineers, I am not formally trained in Machine Learning. For most of my working life, I built compilers, program analysis and programming tools, and IDEs; something very different from Machine Learning. But while working at Microsoft Research, I saw my colleagues applying Machine Learning and statistics to solve hard program analysis and software engineering problems. The success of these techniques allured me. But let's examine: is it really worth your while or is all this buzz just a passing fad?

The four most common buzzwords are Artificial Intelligence (AI), Machine Learning (ML), Deep Learning (DL), Data Science (DS). Let's check out [Google Trends](https://trends.google.com/trends/explore?date=all&q=Machine%20Learning,Data%20Science,Artificial%20intelligence,Deep%20Learning){:target="_blank" rel="nofollow"}

![Google Trends for Artificial Intelligence (AI), Machine Learning (ML), Deep Learning (DL), Data Science (DS)](https://4.bp.blogspot.com/-vAkyT8B2rnA/XjUhdPqHgoI/AAAAAAAAS6Y/SHLFtsK-CiQfEFkr6UL6mexBcX3GIkf1gCKgBGAsYHg/s1600/ml-trek-google-trends.png){: width="100%" class="framedimg"} for these phrases, as the saying goes: In God we trust; others must provide data.

Since AlexNet in September 2012, there has been a significant increase in these four phrases. [AlexNet winning ImageNet competition by a margin of almost 11%](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf){:target="_blank" rel="nofollow"} was a watershed moment. In research, improving the state of art by a few percentage points is a big deal, but doing it by over 10% is very rare.

In 2015, [Microsoft Research CNN surpassed human-level vision performance](https://arxiv.org/pdf/1512.03385.pdf){:target="_blank" rel="nofollow"}. Considering focal adjustment and color cones bless human eyes with one of the best depth and color perception in the animal kingdom, beating human-level in vision tasks was astonishing!

At present, it is not just the trends data. In fact, consumers use ML several times every day: web search result ranking, email spam detection, Estimated Arrival Time (ETA) in maps, news story clustering around topics, ads on Google and Facebook, recommendation systems on sites like Amazon and Netflix, and Alexa and Google Assistant.

---

Businesses are also increasingly relying on ML for fraud detection, pricing/financial modeling, customer churn prediction, equipment failure prediction, network intrusion detection, customer segmentation, sentiment classification, image/video analysis, and speech/audio processing.

It has exploded into our daily lives because of two reasons:

- a lot of data is being generated, and
- the availability of economical on-demand cloud computing.

It is clear that ML is not just hype or passing fad. It is real, it is here and now. So the journey is worth it.

---

## Study the Terrain

Once you have decided to go on this journey, it is important to study the terrain. Let's start with understanding the four buzz phrases.

**Artificial intelligence (AI):** intelligence demonstrated by machines, in contrast to the natural intelligence displayed by humans.

**Machine learning (ML):** use of statistical models to perform a specific task without using explicit instructions, relying on patterns and inference instead.

**Deep learning (DL):** machine learning methods using artificial neural networks.

**Data science (DS):** a multidisciplinary field of building systems to extract knowledge and insights from (big amount of) structured and unstructured data. In my opinion, it is a fancy name for statistics + programming.

AI is the broadest term including statistical as well as other techniques. ML is a subset of AI, and DL is a subset of ML. Data Science overlaps with AI, ML, and DL, but has more emphasis on programming compared to the other three. Though that is changing. Data management and continuous deployment of ML/DL models are becoming mainstream. Companies are making significant investments in programming and engineering discipline.

![Relationship between Artificial Intelligence, Machine Learning, Deep Learning and Data Science](https://3.bp.blogspot.com/-lp5okaKB1zs/XjUjHd8duPI/AAAAAAAAS6k/IuMbrGXgH8sfz1LkvnD5p3D1zTsSrpQygCKgBGAsYHg/s1600/ml-trek-AI-ML-DL-DS-relationships.png){: width="100%" class="framedimg"}

### Traditional Programs vs. Machine Learning

All these definitions are fine, but what is the difference between Machine Learning and traditional programs?

In **traditional programming**, a programmer designs logic or algorithm to solve a problem. The program applies this algorithm to input and computes the result.

But in **machine learning**, the programmer does not write the logic to compute results. Instead, she builds a model from the data. The model is logic. As newer data (user feedback on the correctness of output) arrives, the model (which is logic) changes too. So program "learns" on its own.

Machine Learning programs have two distinct phases:

- **Training:** Input and the expected output are used to train and test various models, and select the most suitable model.
- **Inference:** The model is applied to the input to compute results. These results are wrong sometimes. A mechanism is built the gather user feedback on such occasions.

This feedback is added to the training data, which in turn leads to improved models. This loop is called [data-pipeline](https://www.satishchandragupta.com/tech/scalable-efficient-big-data-analytics-machine-learning-pipeline-architecture-on-cloud.html){:target="_blank"} or data-engineering.

![Traditional programs vs. Machine Learning](https://1.bp.blogspot.com/-Fl-Fk2YOe1U/XjUjebmLhQI/AAAAAAAAS6s/fGzJEXGr4LwfaZ4l2rsixJiu9s5QrrTtwCKgBGAsYHg/s1600/ml-trek-traditional-program-vs-machine-learning.png){: width="100%" class="framedimg"}

All this is still abstract. Let's take the problem of detecting email spam, and compare traditional and machine learning solutions.

In **Traditional Programming Solution**, a programmer will analyze how a human will determine whether an email is spam, and enumerate an exhaustive list of rules and patterns. For example:

- the word FREE occurs several times
- there are phrases like Weight Loss, 
- messages claiming you have won a lottery
- messages originating from specific countries or IP addresses, and so on.

As spammers change tactics, the programmer needs to continuously update these rules to keep up with. This is how Knowledge or Expert Systems were built in the past.

In **Machine Learning Solutions**, a programmer will:

- Prepare data set: a large number of emails labeled by humans as spam or no-spam
- Train, test and tune models, and select the best.
- During inference, this model is applied to determine whether to keep in an email in the inbox or the spam folder.
- Statistical models are not 100% accurate. Spammers too keep coming up with new tactics. So sometimes spam classification is incorrect. Users will move such emails from the inbox to the spam folder (or vice versa).
- Such user actions are tracked and are treated as new human-labeled data.
- These examples are added to the data set, and a new model is trained to remain up-to-date with the spam trends.

### Machine Learning

Machine Learning has three kinds of techniques:

**Supervised Learning:** Train a function that maps an input to output. Training infers the relationship in given labeled input-output pair examples (called training dataset). Two common methods are regression and classification.

**Unsupervised Learning:** Train to find previously unknown patterns in data set without pre-existing labels. Two common methods are clustering and principal component analysis (also known as dimensionality reduction).

**Reinforcement Learning:** Train software agents to take actions in an environment to maximize some notion of cumulative reward. Its applications are in robotics, games, skill learning and adaptation.

Let's learn a bit more about the three most common techniques: regression, classification, and clustering.

![Linear Regression](https://3.bp.blogspot.com/-cLErWrxuF1Y/XjUkYY4kmiI/AAAAAAAAS64/bMsycwtUZu4YJmWwlYlZDxoMT6Wy-sW5QCKgBGAsYHg/s1600/ml-trek-regression.png){: width="25%" class="framedimg alignleft"}

**Regression** is a supervised learning technique to estimate the value of a dependent variable from one or more independent variables. An example is to estimate the value of a house from its size, location, number of bedrooms, number of bathrooms, etc.

It is like fitting a curve over the given points such that the difference between the estimated value and the actual value is minimized over a large sample data set. In the estimator function, Y = f(X), Y is called *outcome*, and X is called *feature vector*.

![Logistic Regression or Classification](https://4.bp.blogspot.com/-tMsZx1ujhZM/XjUkYUW7-RI/AAAAAAAAS64/STiUtMS1RxwTmZSJZarouwylW2kWLnTrwCKgBGAsYHg/s1600/ml-trek-classification.png){: width="25%" class="framedimg alignleft"}

**Classification** is a supervised learning technique to identify class/group from the characteristics of an object. An example is to identify whether the vehicle in a given photo is of a car, or a truck, or a motorcycle.

It like drawing lines to divide a region into a number of regions (3 in this case, representing car, truck, motorcycle) such that the number of objects not falling in their region is minimum. In the classification function, Y = f(X), Y is called *labels* (and is a finite set, like an enum in programs), and X is called *feature vector*.

![Clustering](https://2.bp.blogspot.com/-YaAIBpDAzMM/XjUkYQR76uI/AAAAAAAAS64/yXtuYMqzJ5kg1AkvsghXZSHXC6AnVbCigCKgBGAsYHg/s1600/ml-trek-clustering.png){: width="25%" class="framedimg alignleft"}

**Clustering** is an unsupervised learning technique to group objects into clusters of similar objects. In other words, objects in a cluster are more similar to each other than those in other clusters, as per given similarity criteria. An example is to cluster similar news articles based on topics.

The basic difference between classification and clustering is that in classification, the label-set is finite and given; but in clustering, the number and definition of the clusters are not known in advance and is inferred from the data, so label-set is neither finite nor given.

Now let's try to map some of the problems listed in the beginning to one of these techniques:

- Web search result ranking/scoring: *Regression*
- Email spam detection: *Classification*
- ETA in maps: *Regression*
- Showing the ad that will maximize earning: *Regression*
- Recommendation systems: *Clustering*

I want to pause for a moment and emphasize that machine learning is not a magic bullet. It all depends on the data you use for the training because, in ML, the data is logic. If you are not careful in collecting and curating the data, ML predictions can be severely faulty. It is known as Garbage In, Garbage Out.

For supervised learning, it matters what you have trained your system for. For example, if you trained a classification system to distinguish between car, truck, and motorcycle, you can not use it to, say, a red car from a blue car. If your problem changes, you have to change labels in training data, and retrain the model.

### Deep Learning

Deep Learning is a subset of machine learning using **Deep Neural Network (DNN)** models. These models have one input layer, one output layer, and several intermediate hidden layers.

There are various specialized network designs suitable for different problems. Some examples are Convolutional Neural Network (CNN), Recurrent Neural Network (RNN), Long Short-Term Memory (LSTM) neural network. We will not get into details of various types of DNNs in this post.

DNNs solve the same problems: regression, classification, clustering, etc. But DNNs are computation-intensive (and thus expensive), these are used for the certain types of data.

Other machine learning techniques usually suffice for structured data. DNNs give better results on unstructured data. There are three kinds of unstructured data that DNNs are commonly used for:

**Vision:** To process images and video data. Common applications are object recognition in images, video summarization.

**Natural Language:** To process text in natural languages. Common applications are sentiment classification, intent recognition, entity recognition, machine translation.

**Speech:** To process voice audio data. Common applications are speech recognition (speech to text) and speech synthesis (text to speech).

![An example of Deep Neural Network (DNN)](https://3.bp.blogspot.com/-nJGVwGyw7Yg/XjUnjv3vFDI/AAAAAAAAS7E/hqn7DLuP4qgXDijdiNdNofh-90Ei2pJ6wCKgBGAsYHg/s1600/ml-trek-DNN.png){: width="100%" class="framedimg"}

### Tools and Frameworks

Python is the most popular language among machine learning practitioners. There is a rich ecosystem of libraries and frameworks.

For data science and machine learning, you can use [NumPy](https://numpy.org/){:target="_blank" rel="nofollow"}, [Pandas](https://pandas.pydata.org/){:target="_blank" rel="nofollow"}, [SciPy](https://www.scipy.org/){:target="_blank" rel="nofollow"}, and [SciKit-Learn](https://scikit-learn.org/){:target="_blank" rel="nofollow"}. For data visualization, [MatplotLib](https://matplotlib.org/){:target="_blank" rel="nofollow"} and [Seaborn](https://seaborn.pydata.org/){:target="_blank" rel="nofollow"} are useful.

During experimentation and exploring ideas, [Jupyter Notebook](https://jupyter.org/){:target="_blank" rel="nofollow"}, [Kaggle kernels](https://www.kaggle.com/kernels){:target="_blank" rel="nofollow"}, and [Google Colabs](https://colab.research.google.com/){:target="_blank" rel="nofollow"} are very convenient to keep notes of the code as well as visualization and experiment outputs.

For deep learning, [TensorFlow](https://www.tensorflow.org/){:target="_blank" rel="nofollow"} and [PyTorch](https://pytorch.org/){:target="_blank" rel="nofollow"}, and [Keras](https://keras.io/){:target="_blank" rel="nofollow"} API are the most popular frameworks for building neural networks.

To deploy on the cloud, there are alternatives from all major cloud providers: Google [Cloud AutoML](https://cloud.google.com/automl/){:target="_blank" rel="nofollow"}, Amazon [SageMaker](https://docs.aws.amazon.com/sagemaker/){:target="_blank" rel="nofollow"}, Microsoft [Azure ML](https://docs.microsoft.com/en-us/azure/machine-learning/){:target="_blank" rel="nofollow"}.

## Train Hard

As you can see, machine learning is a vast terrain, a lot of ground to cover. And it is not easy. So you need to train hard.

The good news is that it is the best time in human history to learn even advance topics of computer science on your own. You just need motivation and a computer with a good internet connection.

There is no dearth of good articles, tutorials, and excellent and affordable online courses. I reiterate, if you are motivated to learn machine learning on your own, there wasn't a better time ever.


### Online Courses, Tutorials, Resources

I am listing a few courses that I am fond of, but there are so many other very good and useful online resources.

- Machine Learning Basics: [Andrew Ng's famous course](https://www.coursera.org/learn/machine-learning){:target="_blank" rel="nofollow"}
- Machine Learning for practitioners: [DS/ML Python Bootcamp @ Udemy](https://www.udemy.com/python-for-data-science-and-machine-learning-bootcamp/){:target="_blank" rel="nofollow"}
- TensorFlow: [Tutorials](https://www.tensorflow.org/tutorials){:target="_blank" rel="nofollow"}
- Deep Learning:  [Courses @ DeepLearning.ai](https://www.deeplearning.ai/){:target="_blank" rel="nofollow"}
- Cloud: Google [Cloud AutoML](https://cloud.google.com/automl/){:target="_blank" rel="nofollow"}, Amazon [SageMaker](https://docs.aws.amazon.com/sagemaker/){:target="_blank" rel="nofollow"}, Microsoft [Azure ML](https://docs.microsoft.com/en-us/azure/machine-learning/){:target="_blank" rel="nofollow"}
- [Kaggle](https://www.kaggle.com/){:target="_blank" rel="nofollow"} has a rich source of data sets and competitions.

### Books

Once you get started in ML, you might want to pick some of these books and strengthen your theory foundation. These are some of the best books from some of the most respected professors. These are available online, free of cost.

- [The Elements of Statistical Learning](https://web.stanford.edu/~hastie/ElemStatLearn/){:target="_blank" rel="nofollow"}, by Hastie, Tibshirani, Friedman
- [An Introduction to Statistical Learning](http://faculty.marshall.usc.edu/gareth-james/ISL/){:target="_blank" rel="nofollow"}, by James, Witten, Hastie, Tibshirani
- [Deep Learning](https://www.deeplearningbook.org/){:target="_blank" rel="nofollow"}, by Goodfellow, Bengio, Courville, Bach
- [Pattern Recognition and Machine Learning](https://www.microsoft.com/en-us/research/people/cmbishop/#!prml-book){:target="_blank" rel="nofollow"}, by Bishop
- [Machine Learning Yearning](https://www.deeplearning.ai/machine-learning-yearning/){:target="_blank" rel="nofollow"}, by Andrew Ng
- [Speech and Language Processing](https://web.stanford.edu/~jurafsky/slp3/){:target="_blank" rel="nofollow"}, by Jurafsky, Martin

## Check Your Gears

As you begin your ML journey, it is time to check your gears and get a hang of the map and compass.

### Beware of blind spots

**Deterministic logic** is ingrained in Software Engineers. But machine learning is statistical in nature. We need to learn to accept that the model will not work correctly on all inputs. Fixing it for a particular input will most likely degrade overall performance. Learning that was the biggest struggle I had.

You may be surprised that a unit test that has been passing, may start failing even without any code change. It can happen due to the random partitioning of training data into the train, validate, and test sets. Random partitioning can result in a slightly different model. And that model can coincidently fail on the input used in that unit test. You need to internalize the notion of **statistical correctness**.

### Map

You learned about the three most prominent machine learning techniques: linear regression, classification, and clustering. You know about neural networks and applications of deep learning. You also got a list of courses and books for in-depth knowledge. If you want to gather a quick glimpse of the landscape, here is a list of articles to quickly start working on a specific technique or problem.

#### Quickstart Guide:

- An [intro](https://towardsdatascience.com/intro-to-data-science-part-1-numpy-and-pandas-49d98740661b){:target="_blank" rel="nofollow"} and [tutorial](https://www.hackerearth.com/practice/machine-learning/data-manipulation-visualisation-r-python/tutorial-data-manipulation-numpy-pandas-python/tutorial/){:target="_blank" rel="nofollow"} to [data manipulation](https://towardsdatascience.com/data-manipulation-for-machine-learning-with-pandas-ab23e79ba5de){:target="_blank" rel="nofollow"} with [NumPy and Pandas](https://towardsdatascience.com/numpy-and-pandas-for-data-scientists-2be4a093b4b5){:target="_blank" rel="nofollow"}
- [Tips](https://towardsdatascience.com/10-tips-to-improve-your-plotting-f346fa468d18){:target="_blank" rel="nofollow"} on [graph plotting](https://towardsdatascience.com/basics-of-graph-plotting-7eaadd11a8d){:target="_blank" rel="nofollow"} and [data visualization](https://towardsdatascience.com/introduction-to-data-visualization-in-python-89a54c97fbed){:target="_blank" rel="nofollow"} with [Matplotlib](https://towardsdatascience.com/data-visualization-using-matplotlib-16f1aae5ce70){:target="_blank" rel="nofollow"} and [Seaborn](https://towardsdatascience.com/data-visualization-using-seaborn-fc24db95a850){:target="_blank" rel="nofollow"}, and having [fun](https://towardsdatascience.com/seaborn-lets-make-plotting-fun-4951b89a0c07){:target="_blank" rel="nofollow"} doing [Exploratory Data Analysis](https://towardsdatascience.com/how-to-perform-exploratory-data-analysis-with-seaborn-97e3413e841d){:target="_blank" rel="nofollow"}
- A [hands-on](https://towardsdatascience.com/hands-on-introduction-to-scikit-learn-sklearn-f3df652ff8f2){:target="_blank" rel="nofollow"} [intro](https://towardsdatascience.com/an-introduction-to-scikit-learn-the-gold-standard-of-python-machine-learning-e2b9238a98ab){:target="_blank" rel="nofollow"} to [Scikit-Learn](https://scikit-learn.org/stable/tutorial/index.html){:target="_blank" rel="nofollow"} with [examples](https://scikit-learn.org/stable/tutorial/statistical_inference/supervised_learning.html){:target="_blank" rel="nofollow"}
- A [beginner’s guide](https://towardsdatascience.com/a-beginners-guide-to-linear-regression-in-python-with-scikit-learn-83a8f7ae2b4f){:target="_blank" rel="nofollow"} to [Linear Regression](https://towardsdatascience.com/linear-regression-detailed-view-ea73175f6e86){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://scikit-learn.org/stable/modules/linear_model.html){:target="_blank" rel="nofollow"}
- [Overview](https://towardsdatascience.com/logistic-regression-detailed-overview-46c4da4303bc){:target="_blank" rel="nofollow"} of [Logistic Regression](https://towardsdatascience.com/logistic-regression-using-python-sklearn-numpy-mnist-handwriting-recognition-matplotlib-a6b31e2b166a){:target="_blank" rel="nofollow"} or [Classification](https://towardsdatascience.com/machine-learning-classifiers-a5cc4e1b0623){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://scikit-learn.org/stable/auto_examples/linear_model/plot_iris_logistic.html){:target="_blank" rel="nofollow"}
- [Overview](https://towardsdatascience.com/support-vector-machine-introduction-to-machine-learning-algorithms-934a444fca47){:target="_blank" rel="nofollow"} and [explanation](https://towardsdatascience.com/support-vector-machine-explained-8d75fe8738fd){:target="_blank" rel="nofollow"} of [Support Vector Machines](https://towardsdatascience.com/tagged/support-vector-machine){:target="_blank" rel="nofollow"} ([SVM](https://towardsdatascience.com/support-vector-machines-svms-4bcccbd78369){:target="_blank" rel="nofollow"}) with [Scikit-Learn](https://scikit-learn.org/stable/modules/svm.html){:target="_blank" rel="nofollow"} and various [kernels](https://stackabuse.com/implementing-svm-and-kernel-svm-with-pythons-scikit-learn/){:target="_blank" rel="nofollow"}
- [Introduction](https://towardsdatascience.com/random-forest-3a55c3aca46d){:target="_blank" rel="nofollow"} to [Decision Tree](https://towardsdatascience.com/decision-trees-in-machine-learning-641b9c4e8052){:target="_blank" rel="nofollow"} and [implementing](https://towardsdatascience.com/an-implementation-and-explanation-of-the-random-forest-in-python-77bf308a9b76){:target="_blank" rel="nofollow"} and [understanding](https://towardsdatascience.com/understanding-random-forest-58381e0602d2){:target="_blank" rel="nofollow"} Enchanted [Random Forests](https://towardsdatascience.com/enchanted-random-forest-b08d418cb411){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://scikit-learn.org/stable/modules/tree.html){:target="_blank" rel="nofollow"}
- [Intro](https://towardsdatascience.com/naive-bayes-classifier-explained-50f9723571ed){:target="_blank" rel="nofollow"}, [Tutorial](https://medium.com/@awantikdas/a-comprehensive-naive-bayes-tutorial-using-scikit-learn-f6b71ae84431){:target="_blank" rel="nofollow"}, [explanation](https://towardsdatascience.com/naive-bayes-classifier-explained-54593abe6e18){:target="_blank" rel="nofollow"}, and [implementation](https://hub.packtpub.com/implementing-3-naive-bayes-classifiers-in-scikit-learn/){:target="_blank" rel="nofollow"} of [Naive Bayes](https://www.datacamp.com/community/tutorials/naive-bayes-scikit-learn){:target="_blank" rel="nofollow"} [Classifier](https://www.sicara.ai/blog/2018-02-28-naive-bayes-classification-sklearn){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://scikit-learn.org/stable/modules/naive_bayes.html){:target="_blank" rel="nofollow"}
- An [overview](https://towardsdatascience.com/an-overview-of-time-series-forecasting-models-a2fa7a358fcb){:target="_blank" rel="nofollow"} of [Time Series](https://towardsdatascience.com/an-end-to-end-project-on-time-series-analysis-and-forecasting-with-python-4835e6bf050b){:target="_blank" rel="nofollow"} data handling and [forecasting](https://link.medium.com/Zj7ldgSbs5){:target="_blank" rel="nofollow"} with Scikit-Learn
- [Understanding](https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229){:target="_blank" rel="nofollow"} the [Bias-Variance tradeoff](https://towardsdatascience.com/the-bias-variance-tradeoff-8818f41e39e9){:target="_blank" rel="nofollow"}
- [Accuracy, Precision, Recall](https://towardsdatascience.com/accuracy-precision-recall-or-f1-331fb37c5cb9){:target="_blank" rel="nofollow"}, and [F1 score](https://towardsdatascience.com/multi-class-metrics-made-simple-part-ii-the-f1-score-ebe8b2c2ca1){:target="_blank" rel="nofollow"}, which [metrics](https://medium.com/@george.drakos62/how-to-select-the-right-evaluation-metric-for-machine-learning-models-part-3-classification-3eac420ec991){:target="_blank" rel="nofollow"} to use for [evaluating ML algorithms](https://medium.com/@george.drakos62/how-to-select-the-right-evaluation-metric-for-machine-learning-models-part-3-classification-3eac420ec991){:target="_blank" rel="nofollow"}
- [Ridge (L2) and Lasso (L1) Regularization](https://towardsdatascience.com/regularization-in-machine-learning-76441ddcf99a){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://towardsdatascience.com/ridge-and-lasso-regression-a-complete-guide-with-python-scikit-learn-e20e34bcbf0b){:target="_blank" rel="nofollow"}
- [K Nearest Neighbors](https://towardsdatascience.com/building-a-k-nearest-neighbors-k-nn-model-with-scikit-learn-51209555453a){:target="_blank" rel="nofollow"} or [k-NN](https://towardsdatascience.com/knn-using-scikit-learn-c6bed765be75){:target="_blank" rel="nofollow"} classifier with [Scikit-Learn](https://scikit-learn.org/stable/modules/neighbors.html), a [tutorial](https://machinelearningmastery.com/tutorial-to-implement-k-nearest-neighbors-in-python-from-scratch/){:target="_blank" rel="nofollow"}
- [K-Means Clustering](https://towardsdatascience.com/k-means-clustering-with-scikit-learn-6b47a369a83c){:target="_blank" rel="nofollow"} [explained](https://towardsdatascience.com/k-means-clustering-explained-4528df86a120){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://scikit-learn.org/stable/tutorial/statistical_inference/unsupervised_learning.html){:target="_blank" rel="nofollow"}
- [DBSCAN](https://towardsdatascience.com/dbscan-algorithm-complete-guide-and-application-with-python-scikit-learn-d690cbae4c5d){:target="_blank" rel="nofollow"} [clustering](https://towardsdatascience.com/dbscan-clustering-for-data-shapes-k-means-cant-handle-well-in-python-6be89af4e6ea){:target="_blank" rel="nofollow"} with [Scikit-Learn](https://scikit-learn.org/stable/auto_examples/cluster/plot_dbscan.html){:target="_blank" rel="nofollow"}
- [Principal Component Analysis](https://towardsdatascience.com/a-one-stop-shop-for-principal-component-analysis-5582fb7e0a9c){:target="_blank" rel="nofollow"} ([Dimensionality Reduction](https://towardsdatascience.com/dimensionality-reduction-for-machine-learning-80a46c2ebb7e){:target="_blank" rel="nofollow"}) with [Scikit-Learn](https://scikit-learn.org/stable/auto_examples/decomposition/plot_pca_3d.html){:target="_blank" rel="nofollow"} [example](https://scikit-learn.org/stable/auto_examples/decomposition/plot_pca_iris.html){:target="_blank" rel="nofollow"}
- A [brief explanation](https://towardsdatascience.com/brief-on-recommender-systems-b86a1068a4dd){:target="_blank" rel="nofollow"} of [Building](https://towardsdatascience.com/how-to-build-a-simple-recommender-system-in-python-375093c3fb7d){:target="_blank" rel="nofollow"} a [Recommender Systems](https://towardsdatascience.com/introduction-to-recommender-systems-6c66cf15ada){:target="_blank" rel="nofollow"}

Now you have a map of the ML landscape. You are ready to go on the journey of learning how ML models work.

### Compass

Data scientists are strong in mathematics, they are expert in playing with data and designing efficient models. These might not be exactly your forte, but you need to slowly develop these capabilities. Without that, a model will remain a black box.

![तमसो मा ज्योतिर्गमय (tamasō mā jyōtirgamaya). Lead me from darkness to light.](https://1.bp.blogspot.com/-nbDcNaZojeY/XjUonNp5bNI/AAAAAAAAS7Q/oB29CVhyR6wHCiDBMS9kTXRd87RRZMGgACKgBGAsYHg/s1600/ml-trek-map-and-compass.png){: width="30%" class="framedimg alignleft"}

Do remember your strengths as a software engineer. You have strong programming skills. You are an expert in building highly scalable applications. You have mastered the continuous develop-test-deploy pipeline. You engineer systems that run 24x7 with automated monitoring and alerting.

These skills are not common in data scientists. They may not care about establishing correctness guarantees of an ML model in a 24x7 production system. They typically build batch programs for a given data set. It may require significant (re)work and engineering to take it to production. You have to adapt your engineer practices to the world of ML. You can bring that engineering discipline and rigor to ML.

Both data scientists and software engineers need to develop a better understanding of the counterpart. They must keep moving towards the top-right quadrant. That is your compass.

## Adventure Begins

![](https://4.bp.blogspot.com/-lNNKCjUkQhI/XjUpLBxj5WI/AAAAAAAAS7Y/GAHYAoY5TxwoAdhzgvdDhCmfrW3ZhRp8QCKgBGAsYHg/s1600/start-trek-may-the-force-be-with-you.png){: width="30%" class="alignright"}

Key takeaways are:

- AI is real, the future is here. As Andrew Ng says: AI is the new electricity. Like electricity during the industrial revolution, AI will revolutionize one industry after another.
- It is only the beginning of AI adventure humanity is embarking upon.
- You can learn anything by training, start today!

*Best of Luck, Bon Voyage !!!*
