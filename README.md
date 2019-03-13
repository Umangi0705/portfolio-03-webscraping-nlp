# Project 3: NLP Classifiers - Reddit API - README.md
#### Thomas Ludlow, General Assembly NY-DSI-6
#### December 21, 2018

## Problem Statement

**How can we use predictive modeling to best predict which subreddit a post came from?**  
**Which words will most differentiate our two chosen subreddits?**

## Executive Summary

This project sets out to propose a classification modeling approach that would most accurately enable the data scientists at Reddit.com to predict which subreddit a post belongs to, using only the title and post data with subreddit names removed.  This will assist in solving the problem case of Reddit's backend data failure and can reduce the amount of manual review required to return each post to its original sub.  To illustrate this process and build a proposal for Reddit, we have selected to build, evaluate, and compare classification models using Natural Language Processing (NLP) tools.  

Our chosen subreddits to compare are:

 - **r/audioengineering**
 - **r/livesound**
 
These subreddits are similar in many ways with overlapping terminology.  However, Audio Engineering is focused more on in-studio processes, while Live Sound is focused on audio for live events.

The following sections are supported by the respective numbered Jupyter Notebooks.

#### 1. API and EDA

The data used to train and test our model sets comes from Reddit's API, which returns `.json` dictionaries for data requests.  We iterated through to collect as many posts as permitted, and obtained from 924 posts from r/audioengineering and 980 for r/livesound.  These were parsed into post and title content and saved to csv using a Pandas DataFrame.

EDA included removing null values and tokenizing using Regex to remove punctuation, url components, and HTML formatted character codes.  For modeling rigor, literal subreddit name references were also removed, though individual words (e.g. "audio", "live") were left in.

#### 2. Pre-Processing

Pre-processing includes stemming, lemmatizing, and vectorizing with Count Vectorizer and TF-IDF (Term Frequency-Inverse Document Frequency) to enhance modeling response.  We divide our data into a Train and Test vector to begin modeling.

#### 3. Model Selection

Using a GridSearchCV hyperparameter optimization, we selected 4 models to build and examine:
 - Multinomial Naive-Bayes
 - Random Forest
 - Gradient Boost Decision Tree
 - Logistic Regression

#### 4. Model Optimization

By iterating through individual GridSearches for each model, we can use trial and error to manually optimize our hyperparameters.  We use a Pipeline to batch our pre-processing and model fitting into a single process, which GridSearch can iterate through.  Our optimized settings for each model are built into our final models.

#### 5. Model Evaluation

We ran each model against its Train/Test data to assess its accuracy score, and we then pulled additional new Reddit data from each sub for testing.  We obtained 79 new posts from r/audioengineering and 48 from r/livesound to test.


## Data Dictionary

The following variables were used in Train, Test, and New datasets:

|Feature|Type|Dataset|Description|
|---|---|---|---|
|**post_lm**|*object*|Train/Test/New|String of full Lemmatized post vector, tokens separated by spaces|
|**post_st**|*object*|Train/Test/New|String of full Stemmed post vector, tokens separated by spaces|
|**is_ls**|*int*|Train/Test/New|Binary variable: "is LS" = 1 for LiveSound, 0 for AudioEngineering|

## Conclusions & Recommendations

The final models run individually and together with VoteClassifier found the following prediction accuracy scores:

Multinomial Naive-Bayes
Train/Test: 83.4%			New Posts: 84.3% 

Random Forest
Train/Test: 77.3%			New Posts: 76.4%

Gradient Boost
Train/Test: 80.5%			New Posts: 81.9%

Linear Regression
Train/Test: 81.3%			New Posts: 85.0%

**Final Model:**
VoteClassifier
Train/Test: 83.0%			New Posts: 88.98%


#### Recommendations

 - **Highest-performing model is Logistic Regression, and coefficients allow us to understand the data easily**
 - **Use ensemble modeling methods like sklearn’s VotingClassifier to improve accuracy**
 - **Develop multiple models for targeted purposes**
     -  Random Forest had the lowest accuracy, but it has 91.7% specificity
 - **Use models to forecast and budget for manual administrator review**
 - **Plan ahead - we can't build a predictive model without sufficient training data**

## Data Sources

Subreddit 1 - 'Audio Engineering': https://reddit.com/r/audioengineering
Subreddit 2 - 'Live Sound': https://reddit.com/r/livesound

StackOverflow Feature Identification for MultiNB: https://stackoverflow.com/questions/11116697/how-to-get-most-informative-features-for-scikit-learn-classifiers

Word Clouds built on https://wordclouds.com/