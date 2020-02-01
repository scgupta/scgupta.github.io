---
title: An Engineer's trek into Machine Learning
excerpt: "A guide for software engineers wanting to learn data science, machine learning,  and deep learning, with intro to the basics and list of online resources."
image: "https://3.bp.blogspot.com/-rCFTH5ZdyC8/XjUYg5ADI5I/AAAAAAAAS5I/eTIQdL9X0HgNhreStCLuAhTSPA1epqtDwCKgBGAsYHg/s1600/star-trek-ScottyMeetsJaylah-StarTrekBeyond.jpg"
image_thumbnail: "https://3.bp.blogspot.com/-R1S1dcdh7EM/XjUZDuzGA9I/AAAAAAAAS5g/aTb4DfSwDTgTdVSY2U87QVLXJCRmFLZgQCKgBGAsYHg/s320/star-trek-ScottyMeetsJaylah-StarTrekBeyond.jpg"
tags:
  - "machine-learning"
---

![](https://3.bp.blogspot.com/-rCFTH5ZdyC8/XjUYg5ADI5I/AAAAAAAAS5I/eTIQdL9X0HgNhreStCLuAhTSPA1epqtDwCKgBGAsYHg/s1600/star-trek-ScottyMeetsJaylah-StarTrekBeyond.jpg){: width="100%" class="framedimg"}

This question is posed to me quite often:

> I am a software engineer. There is buzz about data science, machine learning, artificial intelligence, etc. all around. How can I learn and get into it? How did you do it?

My story is that I got ambushed and crash-landed into it like engineer Scotty of USS Enterprise in the movie Star Trek Beyond. Later, I got rescued by aliens and they taught me how to survive in the wild. It's true! Seriously!

You see, for most of my working life, I have built compilers, program analysis and programming tools, and IDEs. While working at Microsoft Research, I saw my colleagues applying machine learning (ML) and statistics to solve program analysis, dev tools, and software engineering problems. The success of these techniques surprised me. AlexNet happened around the same time. I was intrigued, and since then I have been learning slowly, but steadily.

That is my story. Now about the first question: how can you learn and get into ML? Apart from programming, I also love to trek in the Himalayas. I see interesting parallels in my ML journey and high altitude treks. Through those parallels, I will explain how you can plan and go on an ML expedition.


# A Trekking Expedition

Typically a trek has following four stages.

**Irresistible Allure:** I am typically drawn to a trek by hearing or reading about it. The peaks, trail, valley, terrain… people gushing that it is a beautiful and challenging trek. It is an allure difficult to resist. But before I invest time, energy and resources, I ask: is the trail worth it? What is the experience like? Why should I go on it? I read people's blog, see photos, spend time on Google Earth. In the end, I either say "Naah!" or I am even more hooked into doing it.

**Study the Terrain:** That is when the real work starts. Every trek has its challenges. So I study the terrain on Google Maps and Google Earth. I spend quite a bit of time looking elevation maps, reading blogs, understanding points of no returns, possible escape routes so that I know key geographical features, difficulties and dangers.

**Train Hard:** Once it is clear what I am up to, I plan and prepare. I start training hard, make a detailed plan, and regularly assess my progress.

**Check your Gears:** Finally, I stock the supplies and check my gears, especially map and compass. I ensure that the map is reasonably ingrained in my mind, and I know enough about the vicinity to have an intuitive sense of direction.

And then the adventure begins!

Let's examine these four stages of the expedition to learn ML.

![My friends and I climbing to Auden's Col in Gangotri National Park, Uttarakhand Himalayas, India](https://3.bp.blogspot.com/-uwoEYY0iUYQ/XjUgU5D2txI/AAAAAAAAS6M/53OLr6ibp08mUScnyoOq5Fl3RtkmKmvzQCKgBGAsYHg/s1600/AudenColClimb.jpg){: width="100%" class="framedimg"}

# Irresistible Allure

I mentioned earlier that I watched some of my colleague using machine learning to solve hard software engineering problems, that is how I was allured. But let's examine: is it worth or is all buzz just a passing fad.

The four most common buzz words are Artificial Intelligence (AI), Machine Learning (ML), Deep Learning (DL), Data Science (DS). Let's check out [Google Trends](https://trends.google.com/trends/explore?date=all&q=Machine%20Learning,Data%20Science,Artificial%20intelligence,Deep%20Learning){:target="_blank" rel="nofollow"} for these phrases, as the saying goes: In God we trust; others must provide data.

![Google Trends for Artificial Intelligence (AI), Machine Learning (ML), Deep Learning (DL), Data Science (DS)](https://4.bp.blogspot.com/-vAkyT8B2rnA/XjUhdPqHgoI/AAAAAAAAS6Y/SHLFtsK-CiQfEFkr6UL6mexBcX3GIkf1gCKgBGAsYHg/s1600/ml-trek-google-trends.png){: width="100%" class="framedimg"}


Since AlexNet in September 2012, there has been a significant increase for these four phrases. [AlexNet winning ImageNet competition by a margin of almost 11%](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf){:target="_blank" rel="nofollow"} was a watershed moment. In research, improving the state of art by a few percentage points is a big deal, but doing it by over 10% is very rare.

In 2015, [Microsoft Research CNN surpassed human-level vision performance](https://arxiv.org/pdf/1512.03385.pdf){:target="_blank" rel="nofollow"}. Considering focal adjustment and color cones bless human eyes with one of the best depth and color perception in the animal kingdom, beating human-level in vision tasks was astonishing!

At present, it is not just the trends data, in fact, we use ML several times every day. Some examples are web search result ranking, email spam detection, ETA in maps, news story clustering around topics, ads on Google and Facebook, recommendation systems on sites like Amazon and Netflix, and Alexa and Google Assistant.

Businesses are also increasingly relying on ML for fraud detection, pricing/financial modelling, customer churn prediction, equipment failure prediction, network intrusion detection, customer segmentation, sentiment classification, image/video analysis, speech/audio processing.

It has exploded into our daily lives because of two reasons: a lot of data is being generated, and the availability of economical on-demand cloud computing. It is clear that ML is not just hype or passing fad. It is real, it is here and now. So the journey is worth it.

# Study the Terrain

Once you have decided to go on this journey, it is important to study the terrain starting with understanding the four buzz phrases.

**Artificial intelligence (AI):** intelligence demonstrated by machines, in contrast to the natural intelligence displayed by humans.

**Machine learning (ML):** use of statistical models to perform a specific task without using explicit instructions, relying on patterns and inference instead.

**Deep learning (DL):** machine learning methods using artificial neural networks.

**Data science (DS):** multidisciplinary field of building systems to extract knowledge and insights from (big amount of) structured and unstructured data. In my opinion, it is a fancy name for statistics + programming.

AI is the broadest term including statistical as well as other techniques. ML is a subset of AI, and DL is a subset of ML. Data Science overlaps with AI, ML and DL, but traditionally had more emphasis on programming compared to the other three. But that is changing. Data management and continuous deployment of ML/DL models is becoming increasingly mainstream, requiring significant investments in programming and engineering discipline.

![Relationship between Artificial Intelligence, Machine Learning, Deep Learning and Data Science](https://3.bp.blogspot.com/-lp5okaKB1zs/XjUjHd8duPI/AAAAAAAAS6k/IuMbrGXgH8sfz1LkvnD5p3D1zTsSrpQygCKgBGAsYHg/s1600/ml-trek-AI-ML-DL-DS-relationships.png){: width="100%" class="framedimg"}

## Traditional Programs vs. Machine Learning

All these definitions are fine, but what is the difference between Machine Learning and traditional programs?

Traditionally, a programmer designs logic or algorithm to solve a problem. The program applies this algorithm to input and computes the result.

However, in machine learning, the programmer does not write the logic for computing results, instead builds a model from the data. The model is logic. As newer data (user feedback on the correctness of output) arrives, the model (which is logic) changes too. So program "learns" on its own.

Machine Learning programs have two distinct phases: Training and Inference. During training, input and the expected output are used to train and test various models, and the most suitable model is picked. During inference, the model is applied to the input to compute results. These results are wrong sometimes. A mechanism is built the gather user feedback on such occasions. This feedback is added to the training data, which in turn leads to improved models. This loop is called data-pipeline or data-engineering.

![Traditional programs vs. Machine Learning](https://1.bp.blogspot.com/-Fl-Fk2YOe1U/XjUjebmLhQI/AAAAAAAAS6s/fGzJEXGr4LwfaZ4l2rsixJiu9s5QrrTtwCKgBGAsYHg/s1600/ml-trek-traditional-program-vs-machine-learning.png){: width="100%" class="framedimg"}

All this is still abstract. Let's take the problem of detecting email spam, and compare how it is solved traditionally and now with machine learning.

In traditional programming, a programmer will analyze how a human will determine whether an email is spam, and enumerate an exhaustive list of rules and patterns. For example, if the word FREE occurs several times, or there are phrases like Weight Loss, or a message that proclaims that you have won a lottery, or originating from specific countries or IP addresses, and so on. As spammers change tactics, the programmer needs to continuously update these rules to keep up with. This is how Knowledge or Expert Systems were built in the past.

In machine learning, a programmer will prepare data set: a large number of emails labeled by human as spam or no-spam. Then, (s)he will train, test and tune models, and select the best. When a new email arrives, this model will be applied to determine to let the mail go to inbox, or move it to the spam folder. Statistical models are not 100% accurate, and also spammers keep coming up with new tactics. So sometimes an email will be wrongly tagged, i.e. moved to the spam folder, which the user will move to inbox; or a spam email delivered into the inbox, which user will move to the spam folder. Such user actions are tracked and are like new human-labeled data. These are added to the data set, and a new model is trained to remain up-to-date with the spam trends.

Now let's understand different kinds of problems that can be solved by machine learning or deep learning techniques.

## Machine Learning

Machine Learning has three kinds of techniques:

**Supervised Learning:** train for a function that maps an input to output by inferring the relationship inherent in the given labeled training dataset, consisting of input-output pair examples. Two common methods are regression and classification.

**Unsupervised Learning:** train to find previously unknown patterns in data set without pre-existing labels. To common methods are clustering and principal component analysis (also known as dimensionality reduction).

**Reinforcement Learning:** train software agents to take actions in an environment in order to maximize some notion of cumulative reward. Its applications are in robotics, games, skill learning and adaptation.

Let's learn a bit more about the three most common techniques: regression, classification, and clustering.

![Linear Regression](https://3.bp.blogspot.com/-cLErWrxuF1Y/XjUkYY4kmiI/AAAAAAAAS64/bMsycwtUZu4YJmWwlYlZDxoMT6Wy-sW5QCKgBGAsYHg/s1600/ml-trek-regression.png){: width="25%" class="framedimg alignleft"}

**Regression** is a supervised learning technique to estimate the value of a dependent variable from one or more independent variables. A classical example is to estimate the value of a house, given the size of the house, location, number of bedrooms, number of bathrooms, etc.

It is like fitting a curve over the given points such that the difference between the estimated value and the actual value is minimized over a large sample data set. In the estimator function, Y = f(X), Y is called outcome, and X is called feature vector.

![Logistic Regression or Classification](https://4.bp.blogspot.com/-tMsZx1ujhZM/XjUkYUW7-RI/AAAAAAAAS64/STiUtMS1RxwTmZSJZarouwylW2kWLnTrwCKgBGAsYHg/s1600/ml-trek-classification.png){: width="25%" class="framedimg alignleft"}

**Classification** is a supervised learning technique to identify class/group from the characteristics of an object. An example is to identify whether the vehicle in a given photo is of a car, or a truck, or a motorcycle.

It like drawing lines to divide a region into a number of regions (3 in this case, representing car, truck, motorcycle) such that the number of objects not falling in their region is minimum. In the classification function, Y = f(X), Y is called labels (and is a finite set, like an enum in programs), and X is called feature vector.

![Clustering](https://2.bp.blogspot.com/-YaAIBpDAzMM/XjUkYQR76uI/AAAAAAAAS64/yXtuYMqzJ5kg1AkvsghXZSHXC6AnVbCigCKgBGAsYHg/s1600/ml-trek-clustering.png){: width="25%" class="framedimg alignleft"}

**Clustering** is an unsupervised learning technique to group objects into clusters of similar objects. In other words, objects in a cluster are more similar to each other than those in other clusters, as per given similarity criteria. An example is to cluster similar news articles based on topics.

The basic difference between classification and clustering is that in classification, the label-set is finite and given; but in clustering, the number and definition of the clusters are not known in advance and is inferred from the data, so label-set is neither finite nor given.

Now let's try to map some of the problems listed in the beginning to one of these techniques:

- Web search result ranking/scoring: *Regression*
- Email spam detection: *Classification*
- ETA in maps: *Regression*
- Showing the ad that will maximize earning: *Regression*
- Recommendation systems: *Clustering*

I want to pause for a moment and emphasize that machine learning is not a magic bullet. It all depends on the data you use for the training, because in ML, the data is logic. So if you do not carefully collect and curate the data, machine learning predictions will be severely faulty. It is commonly known as "Garbage In, Garbage Out."

For supervised learning, it matters what you have trained your system for. For example, if you trained a classification system to distinguish between car, truck and motorcycle, it will not suddenly start differentiating, say, a red car from a blue car, just because now you want to. If your problem changes, labels in training data have to reflect that change, and you have to retrain the model.

## Deep Learning
Deep Learning is a subset of machine learning using Deep Neural Network (DNN) models. These models have one input layer, one output layer, and several intermediate hidden layers. There are various specialised network designs suitable for different problems. Some examples are Convolutional Neural Network (CNN), Recurrent Neural Network (RNN), Long Short-Term Memory (LSTM) neural network. We will not get into details of various types of DNNs in this post.

Problems being solved by DNNs are same: regression, classification, clustering etc. But DNNs are computation-intensive (and therefore expensive), so when do we use DNN? The difference is in the type of data being processed. While other machine learning techniques usually suffice for structured data, DNNs give better results on unstructured data. There are three kinds of unstructured data that DNNs are commonly used for: vision, natural language, and speech.

**Vision:** To process image and video data. Common applications are object recognition in images, video summarization.

**Natural Language:** To process text in natural languages. Common applications are sentiment classification, intent recognition, entity recognition, machine translation.

**Speech:** To process voice audio data. Common applications are speech recognition (speech to text) and speech synthesis (text to speech).

![An example of Deep Neural Network (DNN)](https://3.bp.blogspot.com/-nJGVwGyw7Yg/XjUnjv3vFDI/AAAAAAAAS7E/hqn7DLuP4qgXDijdiNdNofh-90Ei2pJ6wCKgBGAsYHg/s1600/ml-trek-DNN.png){: width="100%" class="framedimg"}

## Tools and Frameworks

Python is the most popular language among machine learning practitioners, and there is a rich ecosystem of libraries and frameworks. 

For data science and machine learning, you can use NumPy, Pandas, SciPy, and SciKit-Learn. For data visualization, MatplotLib and Seaborn are useful. During experimentation and exploring ideas, Jupyter Notebook and Google Colabs are very convenient to keep notes of the code as well as visualisation and experiment outputs.

For deep learning, TensorFlow and PyTorch (and Keras API) are most popular for building neural networks.

To deploy on the cloud, there are alternatives from all major cloud providers: Google Cloud AutoML, Amazon SageMaker, Azure ML.

# Train Hard

As you can see, machine learning is a vast terrain, a lot of ground to cover. And it is not easy. So you need to train hard. Good news is that it is the best time in human history to learn even advance topics of computer science on your own. You just need motivation and a computer with a good internet connection. There is no dearth of good articles, tutorials, and excellent and affordable online courses. I reiterate, if you are motivated to learn machine learning on your own, there wasn't a better time ever.

## Online Courses, Tutorials, Resources

I am listing a few courses that I am fond of, but there are so many other very good and useful online resources.

- Machine Learning Basics: [Andrew Ng's famous course](https://www.coursera.org/learn/machine-learning){:target="_blank" rel="nofollow"}
- Machine Learning for practitioners: [DS/ML Python Bootcamp @ Udemy](https://www.udemy.com/python-for-data-science-and-machine-learning-bootcamp/){:target="_blank" rel="nofollow"}
- TensorFlow: [Tutorials](https://www.tensorflow.org/tutorials){:target="_blank" rel="nofollow"}
- Deep Learning:  [Courses @ DeepLearning.ai](https://www.deeplearning.ai/){:target="_blank" rel="nofollow"}
- Cloud: Google [Cloud AutoML](https://cloud.google.com/automl/){:target="_blank" rel="nofollow"}, Amazon [SageMaker](https://docs.aws.amazon.com/sagemaker/){:target="_blank" rel="nofollow"}, Microsoft [Azure ML](https://docs.microsoft.com/en-us/azure/machine-learning/){:target="_blank" rel="nofollow"}
- [Kaggle](https://www.kaggle.com/){:target="_blank" rel="nofollow"}

## Books

Once you get familiar, you might want to pick some of these books for strengthening your foundation in theory. These are some of the best books from some of the most respected professors, and are available online, free of cost, legally.

- [The Elements of Statistical Learning, by Hastie, Tibshirani, Friedman](https://web.stanford.edu/~hastie/ElemStatLearn/){:target="_blank" rel="nofollow"}
- [An Introduction to Statistical Learning, by James, Witten, Hastie, Tibshirani](http://faculty.marshall.usc.edu/gareth-james/ISL/){:target="_blank" rel="nofollow"}
- [Deep Learning, by Goodfellow, Bengio, Courville, Bach](https://www.deeplearningbook.org/){:target="_blank" rel="nofollow"}
- [Pattern Recognition and Machine Learning, by Bishop](https://www.microsoft.com/en-us/research/people/cmbishop/#!prml-book){:target="_blank" rel="nofollow"}
- [Machine Learning Yearning, by Andrew Ng](https://www.deeplearning.ai/machine-learning-yearning/){:target="_blank" rel="nofollow"}

# Check Your Gears

As you about to begin your journey of mastering machine learning, it is time to check your gears, and get a hang of the map and compass.

As software engineers, deterministic logic has been ingrained in us, but machine learning is statistical in nature. We need to learn to accept that the model will not work correctly on all inputs, and fixing it for a particular input will most likely degrade overall performance. Learning that was the biggest struggle I had. It was a surprise to me that a unit test that was passing in the past, may fail in future even if no code has changed. That can happen, for example, due to random partitioning of training data into train, validate, and test sets; resulting into a slightly different model, which coincidently fails on the input used in that unit test. You need to internalize the notion of statistical correctness.

![तमसो मा ज्योतिर्गमय (tamasō mā jyōtirgamaya)](https://1.bp.blogspot.com/-nbDcNaZojeY/XjUonNp5bNI/AAAAAAAAS7Q/oB29CVhyR6wHCiDBMS9kTXRd87RRZMGgACKgBGAsYHg/s1600/ml-trek-map-and-compass.png){: width="30%" class="framedimg alignleft"}

Data scientists are strong in mathematics, they are expert in playing with data and designing efficient models. These might not be exactly your forte, but you need to slowly develop these capabilities. Without that, a model will remain a black box. Improving the understanding of workings of ML models (with the help of courses and books listed earlier) and adapting engineer practices to ML is your map.

At the same time, do remember your strengths as a software engineer. You have strong programming skills, you are expert in building highly scalable applications with continues develop-test-deploy pipeline that runs 24x7 with automated monitoring and alerting. Data scientists might not necessarily appreciate these aspects and the need for establishing guarantees of continuous functioning and correctness of ML models in production systems. Typically they built batch programs for a given dataset, which may require significant (re)work and engineering to take it to production. So your skills complement data scientists and are highly valuable. You just have to adapt those to the world of machine learning, and infuse engineering discipline and rigor into it. Both data science and software engineers need to develop a better understanding of the other and keep moving towards top-right quadrant. That is your compass.

# Adventure Begins

![](https://4.bp.blogspot.com/-lNNKCjUkQhI/XjUpLBxj5WI/AAAAAAAAS7Y/GAHYAoY5TxwoAdhzgvdDhCmfrW3ZhRp8QCKgBGAsYHg/s1600/start-trek-may-the-force-be-with-you.png){: width="30%" class="alignright"}


Here are three key takeaways. First, AI is real, the future is here. As Andrew Ng says: AI is the new electricity; just like electricity during the industrial revolution, AI is going to revolutionize one industry after another. Second, it is just the beginning of AI adventure humanity is embarking upon. And third, you can learn anything by training, start today!

*Best of Luck, Bon Voyage !!!*
