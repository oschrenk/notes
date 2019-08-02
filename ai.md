## Artificial Intelligence

A broad term for getting computers to perform human tasks. That scope is constantly changing and re-defined.

Today we have *narrow AI* - a system that can do one thing (or a few) better than humans (recognize an object, play chess, …). This still needs training by human.

Common use cases
* Object recognition
* speech regocnition, sound detection
* natural language processing / sentiment analysis
* Prediction
* Translation
* restorsation/transformation (using ML to figure out what should be there)

# Machine Learning
An approach to achieve AI through systems than can learn from experience to find patterns in a set of data

It involves teaching a computer to recognize patterns by example, rather than programming it with specific rules.

All about predicting stuff.

1. Take some data
2. Learn pattern from data
3. Classify unseen data based on knowledge from step 2

The power of ML is that it learns from itself from the data passed to it. The same ML system canoe reused (with enough examples) without rewriting the code. That is powerful

It will change traditional programming with explicit rules, to machine learning programs which learn rules by examples.

# Deep Learning
A technique for implementing  ML.

One DL technique is a concept of deep neural networks (DNNs). It loosely mimics the layer arrangments on a human brain, learning patterns of patterns.

# How to choose data to train ML systems
Features/attributes are used to train an ML system. They are the properties you are trying to learn about

Take fruit as an example and using two attributes: Weight, color. Draw it on a 2d graph. You are now able to draw a line between apple and oranges.

Choosing the inout feature is important. For instance ripeness and number of seeds might not help us identify an orange and distnuihsh it fro other fruits.

Most problems in ML have 20+ dimensions. While harder to visualize, the basic ideas stay the same.

Adding dimensions can often help, but too many dimensions can lead to overfitting. It won’t be able to generalize new things well.

The biggest challenge. *finding enough unbiased data*. Finding 10000 cat pictures might be easy, but some other dat might be harder to come by.

*An ML system can not predict stuff it does not know about*. It will get you the closest match. If your system only knows about dogs and chickens it will classify cows as dogs.

# How ML systems are trained
## Supervised learning
Provide ML system with already labeled data. This is the most studied field.

## Unsupervised learning
Machine must learn from unlabelled data set

Machine must realize itself that there are different number of clusters (the number might not be known ahead of time) and hard to distinguish.

## Reinforcement learning
Learning by trail and error through reward or punishment.

eg. Playing a game rewarding good moves

# But how?
## (Linear) regression
Draw a Lin e through points with best fit. That way you can predict for a given x.
## Draw line between clusters
## k-nearest neighbor
## Neural Network
### Articicial  neuron (perceptron)
### Multi layered perceptrons
### Deep Neural Network (DNN)
Layers between input and output. Each layer learns from the one before
### Convolutional Neural Networks

## ML output
* Regression. Predict numerical values.
* Classification. One of n labels (cat, dog)
* Clustering. Most similar other examples (eg. Related products on amazon)
* sequence prediction (what comes next)

## Types of ML algorithms
* Regression. Eg linear regression
* Instance Based eg. K nearest neighbor
* decision trees. Eg conditional decision trees
* bayesian. Eg naive babes
* clustering. eg k means
* association rules. Apriori algorithm
* artificial neural network. Eg perceptron
* deep learning. Eg multi layered perceptron
* dimensionality reduction. Eg principal component analysis
* ensemble. Eg boosting

[Jason’s Machine Learning 101 - Google Presentaties](https://docs.google.com/presentation/d/1kSuQyW5DTnkVaZEjGYCkfOxvzCqGEFzWBy4e9Uedd9k/edit#slide=id.g1e301fae90_1_576)

[But what *is* a Neural Network? | Chapter 1, deep learning - YouTube](https://www.youtube.com/watch?v=aircAruvnKk)
[How Convolutional Neural Networks work - YouTube](https://www.youtube.com/watch?v=FmpDIaiMIeA)


# Other resources
[Machine Learning | Coursera](https://www.coursera.org/learn/machine-learning)
[deeplearning.ai](https://www.deeplearning.ai/)
https://web.stanford.edu/~jurafsky/NLPCourseraSlides.html

#machinelearning
#ai
#learning
#oschrenk Artificial Intelligence #

Stanford started an [Online course for AI](https://www.ai-class.com). Register [here](https://www.ai-class.com/registration/)

- [Google Group](https://groups.google.com/forum/#!forum/stanford-ai-class)
- [Reddit](http://www.reddit.com/r/aiclass)
