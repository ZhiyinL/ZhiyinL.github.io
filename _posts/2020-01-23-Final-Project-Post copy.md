---
layout: post
Title: Breast Cancer Treatment Recommendation Project

In today's medical system, I identified a huge problem in doctor-patient communication. I think that, for patients, there lacks accessible numbers to support the treatment decisions given by doctors. Often times, patients are left to make decisions between two recommended treatments, but the tradeoff between the two is not quantatitively presented to patients. One main grey area is survival information. For example, in terms of breast cancer, while it is clearly marked that one treatment involves removal of entire breast while the other only removes the lump, patients are not provided how much longer one can survive by choosing the former one compared to choosing the latter one. Therefore, I decide to use the machine learning techniques to provide quantative support for patients. I plan to build a personalized breast cancer treatment recommendation system based on patient survival information. The model is inputted with clinical, genomic, and treastment features and is trained to regress to number of survival months. Therefore, for each treatment combination, the trained model is able to predict a corresponding survival months. If we iterate through all potential baseline treatment options and provide the corresponding survival informations, then we are able to recommend the top three treatment options supported with quantative survival information. 

After researching on the internet, the only dataset that involves breast cancer treatment and survival information is METABRIC dataset. Therefore, I decided to use METABRIC dataset, which is freely available on cbioportal. The dataset contains clinical, genomic, treatment, and survival (number of survival months) information of over 2000 patients. The recorded treatments are chemotherapy, radio therapy, hormone therapy, and breast surgery. 
<img src="/images/treatment.png" width="600"/>
Moreover, there are around 20 clinical features and over 20,000 genomic features. Here's a visual display of the clinical features using pandas dataframe.s
<img src="/images/metabric.png" width="600"/>

For data preprocessing, I started with basic cleaning by omitting repetitive values and applying one hot encoding ordinal and nominal values. Here're detailed explanations for clinical features. 
<img src="/images/continuous.png" width="600"/>
<img src="/images/categorical.png" width="600"/>

In terms of missing values, I created an extra is_nan column for each feature to signal the models the existance of missing values. Then I filled out all missing values with zeros.

I split the dataset into train, validation, and test set with a ratio of 6:2:2.

To avoid the curse of dimensionality, I applied feature selection techniques on clinical and genomic features. On clinical data, I computed pearson correlation values as shown below, then I selected the top 7 features out of the 85 features and included all treatment features for future input. <img src="/images/Pearson_Corr.png" width="600"/>

Due to the lack of available samples, it is necessary to apply data augmentation to generate more samples for training. Analogous to the dropout technique for image dataset, I augment by removing certain amount of features for each sample. I tried two augmentation methods. The first one involving removing certain percentage of missing values for each sample when duplicating. The specific method is stated below.
<img src="/images/aug1.png" width="600"/> 
To prevent the network from relying on and learning excessively from the missing values, I adopted the second augmentation method to create more variability in terms of percentage of dropping values.
<img src="/images/aug2.png" width="600"/>

Before I pickled everything, I applied normalization on train, validation, and test set.

In terms of algorithms, I tried linear regression, polynomial regression, gradient boosting regression, random forest regression with grid search for hyperparameters. However, the Mean Square Error on test set centers around 5000 month^2, which corresponds to an absolute value of 6 years.

I also constructed neural network with the network architecture shown below. I used mean square error loss for the regression problem. 
<img src="/images/neuralnet.png" width="600"/>
However, I encountered a typical problem in the realm of deep learning --- overfitting. Since the learning ability of the neural network is extremely powerful, the network tends to memorize everything in the training set instead of generalizing and learning patterns from it, which causes the network to perform terribly on the test set (with MSE = 8000 month^2 initially). As shown in the plots of loss versus time for train set and validation set, the training loss goes down hugely and the validation loss rises slightly (this an example with 20 epochs). 
<img src="/images/Train(WithAug)_Test(NoAug).png" width="600"/>

Therefore, I reduced the network width and depth, reduced number of epochs, applied the data augmentation, and used the feature selection method. In the end, I resolved the overfitting issue and gained a plot as shown below, in which validation loss goes down with the train loss.
<img src="/images/nooverfit.png" width="600"/>
However, the results on test sets are unpromising (MSE = 5000 month^2), which makes sense since the validation loss, as a mock test loss, also stays around 5000 month^2 in the plot. Applying the neural network to the train and test set shown below respectively, I conclude that the neural network is not able to learn much in this dataset with regression.
<img src="/images/train.png" width="600"/>
<img src="/images/test.png" width="600"/>

Therefore, I decided to turn it into a classification problem that predicts the 5 year survivability of a patient. The five year cut off has a medical significance in which surviving more than five years indicates the patient is being cured while being deceased in less than five years means the patient is not cured. Based on the number of survival months for each patient, I gained labels of 5 year survivability for each patient. The classification problem is similar to the regression problem stated above except that I added an extra step to deal with class imbalance. There's a 1 to 4 ratio between deceased class and alive class. Thus, I upsampled the minority class by replication.

For classification, I ran tests with algorithms including support vector machine, KNN, Naive Bayes, Gradient Boosting with and without cross validation, all of which got similar results (around 75% accuracy). Moreover, I constructed neural network with similar architecture and hyperparameters and used MSE Loss function (which is allowed since this is a binary classification). Moreover, I did 5-fold cross validation and gained the following result. <img src="/images/confmatrix.png" width="600"/>
Other algorithms gained similar results, in which the f1 score for deceased class is terrible and that for alive class is pretty decent. If the model predicts everything to be alive, techinically, the model is able to gain an 76% accuracy. Therefore, it seems like the classifier is also not able to learn and generalize from the dataset. 

Therefore, despite my enormous frustration, I will pivot to another dataset that does not include treatment and try to solve the problem. 

Thank you to my mentor for his awesome support! 

To be continued......


