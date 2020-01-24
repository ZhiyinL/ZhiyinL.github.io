---
layout: post
Title: Final Project Daily Log
---

Below is a log of my daily progress, including:

1.) What I worked on today and What I learned

2.) Any road blocks or questions that I need to get answered

3.) What my goals are for tomorrow

**January 7th**
1) Today I worked on improving the performance of my regression model. I have been stuck at this problem for two weeks to figure out why the performance of my models are not good. I tried ways of different augmentation (including augmenting train set only, augmenting train, validation, and test set, and not augmenting at all), and I plotted the loss versus time for train set and validation set. As seen below, in all three cases, the training loss down hugely and the validation loss rises slightly (I trained for 20 epochs as an example). 
<img src="/images/Train(NoAug)_Test(NoAug).png" width="600"/>
<img src="/images/Train(WithAug)_Test(NoAug).png" width="600"/>
<img src="/images/Train(WithAug)_Test(WithAug).png" width="600"/>
This suggests that the neural network is overfitting, meaning it merely memorizes the answer of each sample rather than learning and generalizing from what's provided in the input. 

Moreover, I tried to tryout different sets of drop rates (to have a more severe dropout rate) for data augmentation Method2 so that it might force my model to generalize and overfit less. However, the results are not promising.  

2) I want to know how to solve this overfitting issue specific to my project, so I went to ask Lauren during my free block. Thank you for Lauren's help. Lauren suggested two things: 1) use a simpler network such as gradient booster 2) try to use SVD to reduce the dimensionality first.

3) My plan for tomorrow. 
a) Dimensionality reduction first and then run the codes
b) Try a simpler model such as gradient booster. 

**January 8th**
I tried to underfit with Augmentation Method 2 by decreasing the width and the depth of the network and increase the augmentation severity. However, the performance of the model is not good at all. Therefore, I officially decided to move towards a new direction: dimensionality reduction to solve this overfitting problem.

I computed the Pearson correlations between each input feature and the target variable and selected the top 15 most correlated input features for future training. From a medical practice standpoint, producing better results from fewer tests is a benefit. Here's a screenshot of the pearson correlation. 
<img src="/images/Pearson_Corr.png" width="600"/>

After dimensionality reduction, I tried a couple of models. I tried linear regression and polynomial regression. The results are not quite promising as suggested by the R^2 values below.
<img src="/images/Pearson_Corr.png" width="600"/>
<img src="/images/Pearson_Corr.png" width="600"/>

 The neural networks perform a MSE = 5774 month^2, which is a really promising result compared to the previous 8000 month^2. 

 I plan to try simpler models tomorrow, including gradient boost, support vector machines, and random forests. 

**January 9th**
I tried simpler models such as gradient boost and random forest. Moreover, I tuned hyperparamters with grid Search to enhance the performance of the model . Also, I shuffled the training points every epoch for neural network. My plan for next is to do SVD for dimensionality reduction, which is an alternate for the Pearson correlation I did. Since medically speaking, interactions between different features in the clinical data also count towards deciding on optimal treatment options. Therefore, it makes since to use SVD. Also, I will try to augment the pearson correlation selected data with method 2. 

**January 10th**
I realized that I should still keep the treatment option features regardless of the dimensionality results. Therefore, we. I also tried SVD for dimensionality reduction instead of Pearson correlation, hoping for better performance of other models. However, the SVD models perform worse than the Pearson correlation models, which is surprisingly disappointing to me. I also tried augmentation method 2 with pearson correlation, and I did not get a much better result for neural network. My plan for next is to turn this into a classification problem.

**January 13th, 14th**
Turning this problem into a classification problem, I tried algorithms including support vector machine, KNN, Naive Bayes, Gradient Boosting with and without cross validation. Most of the models, except for Naive Bayes, got similar results (around 75% accuracy). However, the model's precision values is low, which means the model is bad at differentiate the survivability of a patient who, in reality, is deceased at 5 year cut off. I think the reason behind it is because the number of samples that are deceased after 5 years in the dataset is less than the number of samples that are alive, which is a classific class imbalance problem. I plan to deal with class balance by duplicating data point on train data. Also, I'm half way through building the neural network classifier, which I plan to finish by this Thursday.

**January 15th**
I dealt with class imbalance using the resample method of sklearn. I improved the f1 score of the minority class even though the overall accuracy drops slightly. Also, I tried random forest algorithm, which has similar results to the other algorithms. I plan to debug my Neural Network.


**January 16th**
I encountered a bug that I could not figure out. I searched online on stackoverflow, pytorch discuss threads, and random posts, but I still could not figure out. Haha, nevermind. Right before I went to bed, I figured it out! It was a bug during training the classifier. Initially, I used the loss function torch.nn.BCELoss(), which does not automatically include a sigmoid function as the activation function. This gives an out of bound error. I solve this by using a different loss function: torch.nn.BCEWithLogitsLoss(). 

**January 17th**