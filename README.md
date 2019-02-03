# README

The code features a multitude of machine learning models
used in isolation and conjunction to create an 
"artificial intelligence" that infers a smartphone user’s 
**severity of depression** (or lack thereof) from **data scraped 
off their phone and social media websites (e.g. twitter, 
instagram), which includes Google GPS data, call and text metadata, 
social media usage data and voluntary voice samples. **

The code provides the machine learning algorithm that a
mobile medical application's server uses. This application
can be installed on an Android phone on the spot, and with 
your consent will pull all data mentioned above and run it 
through the machine learning algorithm developped in this 
code repository to give you a prediction for the severity 
of your depression, if there is any to begin with.

The performance of the machine learning algorithm is discussed
in detail in the paper below.

https://digitalcommons.wpi.edu/cgi/viewcontent.cgi?article=3433&context=mqp-all

Cite as:
Dogrucu, A., Perucic, A., Isaro, A., & Ball, D. C. (2018). Sensing Depression


## THE NITTY GRITTY

The entirety of the project is contained in "MLIntP3.ipynb" and "Mlearn.ipynb"

MLIntP3.ipynb contains code that turns raw data into a feature matrix, and some preliminary sklearn experiments.

Mlearn.ipynb houses all of the machine learning code, contains a lot of IPython cells, all labeled with comments describing the experiment done in each and every one of them.

The project is written using Python 3.5.3.

Dependencies are the following:

* requests (2.18.14)
* opencv-python (3.3.0.10)
* urllib (1.22)

Native python imports can be seen in the code.

Usage of MLIntP3.ipynb's Loader, Generator and Featurizer classes are as follows:

```python
l = Loader()
l.downloadAndLabel()

g = Generator(l.filedir, l.lids(), l.lits())
g.generateMatrix()
```

outputs g.featureMatrix


This code is explained below.


## THE ARCHITECTURE OF FEATURIZATION

The code that turns data into matrix of features and labels is contained within
3 major classes.

These are named Loader, Generator, and Featurizer.

### Loader

The "downloadAndLabel" method saves all user data from the scraping server in a
timestamped folder, with every json object(s) for every modality being saved in
a pickle, which is Pythons built in way of (de)/serializing an object.

### Featurizer

The Featurizer is home to an array of featurizing functions that take a pickle
file and return ONE feature for ONE person.

It also converts pickle files of label data into label vectors

The current list of featurizing functions (and their output dimensions) are clearly denoted in code.


### Generator

The generator class generates column vectors for each feature. Say for daily text
frequency, it generates a vector of dimension (1, total number of people).

It generates these column vectors for every feature by calling methods from the
Featurizer class, and appends them together to create a matrix.

This matrix is generated so that the features and labels can be easily divided
into training and testing sets, and experimentation on the whole featureset
is made easier.


## MACHINE LEARNING EXPERIMENTS

In MLIntP3.ipynb, there are multiple cells containing machine learning
experiments. Most of these experiments are conducted to try different families
of regressors and classifiers on our dataset and see if they perform well.

A variety of feature selection methods are also evaluated.

The following methods' successes on our training set are evaluated:
* Naive Bayes
* Logistic Regression
* Random Forest Regressor, Classifier
* SVC, SVR (SVM)
* Basic Neural Net
* Linear Regression
* Randomized Lasso
* Recursive Feature Elimination with CV
* Stratified K-fold CV
* Chi^2 test


In Mlearn.ipynb, the cell labeled "PREPROCESSING" contains the preprocessing
function. It is called "PPer" and its usage is as follows:

```python
# returns data that is normalized, labels as PHQ-9 values for audio data
data, label = PPer(train_data, "au", continuous)

# returns data that is normalized, labels as 1 or 0, 1 being
# phq9 score > specified cutoff
data, label = PPer(train_data, "au", cutoffunbalanced)

# returns data that is normalized, labels as  1 or 0, 1 being
# phq9 score > specified cutoff
# balances the dataset so that there is an equal number of 1 and 0s as labels
data, label = PPer(train_data, "au", continuous)
```

Below "PPer" is a congloremeration of cells, each labeled with a comment in the
beginning, describing the machine learning done below.


The important cells have following labels:
* CLASSIFICATION FINAL TRAINING WITH BAGGING AND SVM
* CLASSIFICATION TEST SET RESULTS
* REGRESSION FINAL TRAINING
* REGRESSION TEST SET RESULTS

To rerun these cell experiments, one should simply run cells labeled
"PREPROCESSING" and "LOADING DATA" before.
