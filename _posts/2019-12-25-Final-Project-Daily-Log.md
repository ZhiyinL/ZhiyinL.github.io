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

**January 10th**

**January 13th**

**January 15th**

**January 16th**

**January 17th**