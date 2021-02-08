# Project: Classify Song Genres from Audio Data Using Different Binary Classifiers

## Introduction

*These recommendations are so on point! How does this playlist know me so well?*

<p align="center">
  <img width="660" height="500" src="https://user-images.githubusercontent.com/67468718/107107282-05f44b80-67e5-11eb-8b8a-669085d9361d.JPG">
</p>

Over the past few years, streaming services with huge catalogs have become the primary means through which most people listen to their favorite music. But at the same time, the sheer amount of music on offer can mean users might be a bit overwhelmed when trying to look for newer music that suits their tastes.

For this reason, streaming services have looked into means of categorizing music to allow for personalized recommendations. One method involves direct analysis of the raw audio information in a given song, scoring the raw data on a variety of metrics. Today, we'll be examining data compiled by a research group known as The Echo Nest. Our goal is to look through this dataset and classify songs as being either 'Hip-Hop' or 'Rock' - all without listening to a single one ourselves. In doing so, we will learn how to clean our data, do some exploratory data visualization, and use feature reduction towards the goal of feeding our data through some simple machine learning algorithms, such as decision trees and logistic regression.

## Datasets

To begin with, let's load the metadata about our tracks alongside the track metrics compiled by The Echo Nest. A song is about more than its title, artist, and number of listens. We have another dataset that has musical features of each track such as <code>danceability</code> and <code>acousticness</code> on a scale from -1 to 1. These exist in two different files, which are in different formats - CSV and JSON. While CSV is a popular file format for denoting tabular data, JSON is another common file format in which databases often return the results of a given query.

Datasets are located in [tracks](https://www.kaggle.com/efstathiasdrolia/fmarockvshiphop) and [echonest_metrics](https://www.kaggle.com/veronikafilippou/echonestmetricsjson).

Let's start by creating two pandas <code>DataFrames</code> out of these files that we can merge so we have features and labels (often also referred to as <code>X</code> and <code>y</code>) for the classification later on.

## Normalizing the feature data

<p>As mentioned earlier, it can be particularly useful to simplify our models and use as few features as necessary to achieve the best result. Since we didn't find any particular strong correlations between our features, we can instead use a common approach to reduce the number of features called <strong>principal component analysis (PCA)</strong>. </p>
<p>It is possible that the variance between genres can be explained by just a few features in the dataset. PCA rotates the data along the axis of highest variance, thus allowing us to determine the relative contribution of each feature of our data towards the variance between classes. </p>
<p>However, since PCA uses the absolute variance of a feature to rotate the data, a feature with a broader range of values will overpower and bias the algorithm relative to the other features. To avoid this, we must first normalize our data. There are a few methods to do this, but a common way is through <em>standardization</em>, such that all features have a mean = 0 and standard deviation = 1 (the resultant is a z-score).</p>

## Principal Component Analysis on our scaled data

<p>Now that we have preprocessed our data, we are ready to use PCA to determine by how much we can reduce the dimensionality of our data. We can use <strong>scree-plots</strong> and <strong>cumulative explained ratio plots</strong> to find the number of components to use in further analyses.</p>
<p>Scree-plots display the number of components against the variance explained by each component, sorted in descending order of variance. Scree-plots help us get a better sense of which components explain a sufficient amount of variance in our data. When using scree plots, an 'elbow' (a steep drop from one data point to the next) in the plot is typically used to decide on an appropriate cutoff.</p>

## Further visualization of PCA

Unfortunately, there does not appear to be a clear elbow in this scree plot, which means it is not straightforward to find the number of intrinsic dimensions using this method.

But all is not lost! Instead, we can also look at the cumulative explained variance plot to determine how many features are required to explain, say, about 85% of the variance (cutoffs are somewhat arbitrary here, and usually decided upon by 'rules of thumb'). Once we determine the appropriate number of components, we can perform PCA with that many components, ideally reducing the dimensionality of our data.

## Models to classify genre

Now we can use the lower dimensional PCA projection of the data to classify songs into genres. To do that, we first need to split our dataset into 'train' and 'test' subsets, where the 'train' subset will be used to train our model while the 'test' dataset allows for model performance validation.

Here, we will be using a simple algorithm known as a decision tree. Decision trees are rule-based classifiers that take in features and follow a 'tree structure' of binary decisions to ultimately classify a data point into one of two or more categories. In addition to being easy to both use and interpret, decision trees allow us to visualize the 'logic flowchart' that the model generates from the training data.

Here is an example of a decision tree that demonstrates the process by which an input image (in this case, of a shape) might be classified based on the number of sides it has and whether it is rotated.

<p align="center">
  <img width="450" height="650" src="https://user-images.githubusercontent.com/67468718/107117039-3e6b4800-682c-11eb-8884-79bcd4b979eb.JPG">
</p>


#### <ins>Models will be used to classify Genres:<ins>

  * **Decision Tree - Entropy - max_depth 3.**
  * **Decision Tree - Gini - max_depth 3.**
  * **Logistic Regression.**
  * **Random Forests.**
  * **Gradient-Boosting.**
  * **XGBoost**
  * **SVM: Support Vector Machines**
  

## SMOTE

SMOTE stands for **Synthetic Minority Over-Sampling Technique**. Just like the name suggests, SMOTE generates a synthetic data for the minority class and then proceeds by joining the points of the minority class with line segments and then places artificial points on these lines:

![SMOTE](https://user-images.githubusercontent.com/67468718/107210218-b0709800-69b8-11eb-97d0-e4a857c6250b.JPG)

The heart of SMOTE is the construction of the **minority classes**. The intuition behind the construction algorithm is simple. You have already studied that oversampling causes overfitting, and because of repeated instances, the decision boundary gets tightened. What if you could generate similar samples instead of repeating them? In the original SMOTE paper (linked above) it has been shown that to a machine learning algorithm, these newly constructed instances are not exact copies, and thus it softens the decision boundary and thereby helping the algorithm to approximate the hypothesis more accurately.

#### Under the hood, the SMOTE algorithm works in below simple steps:
  * Choose a minority class input vector.
  * Find its k nearest neighbors (k_neighbors is specified as an argument in the SMOTE() function).
  * Choose one of these neighbors and place a synthetic point anywhere on the line joining the point under consideration and its chosen neighbor.
  * Repeat the steps until data is balance.
  * SMOTE is implemented in Python using the imblearn library.
  

  
  











