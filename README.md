# README


## THE NITTY GRITTY

The entirety of the project is contained in "MLIntP3.ipynb"

The project is written using Python 3.5.3.

Dependencies are the following:
	
* requests (2.18.14)
* opencv-python (3.3.0.10)
* urllib (1.22)

Native python imports can be seen in the code.



## THE ARCHITECTURE

The code is contained within 3 major classes.

These are named Loader, Generator, and Featurizer.

### Loader

The Loader class is used to request data from the scraping server, so that the 
data can be held locally on a VM.

The "downloadAndLabel" method saves all user data from the scraping server in a 
timestamped folder, with every json object(s) for every modality being saved in
a pickle, which is Pythons built in way of (de)/serializing an object.

### Featurizer

The Featurizer is home to an array of featurizing functions that take a pickle 
file and return ONE feature for ONE person.

It also converts pickle files of label data into label vectors

The current list of featurizing functions (and their output dimensions) are 
listed:

Features

* (1) daily text frequency
* (1) daily call frequency
* (300) twitter word2vec bag of words
* (1) twitter following count
* (1) twitter follower count
* (1) twitter post like frequency
* (1) twitter post retweet frequency
* (1) # of contacts in contactbook
* (1) insgtagram following count
* (1) instagram follower count
* (1) instagram filters used per day
* (8) instagram particular filter usage vector  
* (1) instagram like frequency
* (1) instagram comment frequency
* (1) instagram post frequency
* (3) instagram average Hue, Saturation, Value
* (1) text word2vec bag of words

Labels

* (9) PHQ9 score for every question
* (1) Sum of all PHQ9 scores

### Generator

The generator class generates column vectors for each feature. Say for daily text
frequency, it generates a vector of dimension (1, total number of people). 

It generates these column vectors for every feature by calling methods from the
Featurizer class, and appends them together to create a matrix.

This matrix is generated so that the features and labels can be easily divided
into training and testing sets, and experimentation on the whole featureset
is made easier. 



## TO-DO for winter break

* Convert all applicable features to averages over 7 or 14 days.
eg. The call frequency Featurizer function output would become a (1x14 
feature vector, where every entry is the call frequency for one day.

* Create functions that prepare word2vec embeddings into features that are 
suitable for RNN or LSTM input.

* Create a StudenLife-esque hierarchy for both raw data and features, to
increase ease of use for future students/faculty.  
















 
