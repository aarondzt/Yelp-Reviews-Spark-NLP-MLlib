# Yelp Reviews Spark NLP MLlib
 
Data from https://www.kaggle.com/yelp-dataset/yelp-dataset

### Summary

* Trying to run Spark locally in a Jupyter Notebook, with the future intention of scaling up analytics using Databricks cluster computing
* NLP preprocessing, tokenization, tf-idf
* Logistic regression to uncover the token featuers associated with one and five-star Yelp reviews
* Framework for business analytics - "What should a restaurant owner focus on to gain more five-star reviews and avoid one-star reviews?

### Description

For my project, I am working with the Kaggle Yelp dataset  - in particular, the file that contains the text of the user reviews. This file is 5.89 GB and contains 8,021,122 reviews. The project requires a dataset of at least 500 MB, which would mean around 680k reviews. However, given the complexity of processing natural language data, a dataset of that size would take far too long to process on a personal computer. Thus, I chose to start by working with only 1% of the dataset, around 12k reviews. For this size, an operation such as count vectorization takes around two minutes, which is about the maximum one could have patience with when building an analytics pipeline. Once I have built a robust pipeline that provides meaningful insights, I will scale up my analysis to 10% to increase my analytical power, with the final intention of processing the entire dataset using cluster computing on a platform such as Databricks. 

The purpose of this project is to use high throughput natural language processing (NLP) on Apache Spark to investigate the features that are associated with one-star versus five-star reviews. First, I loaded the entire reviews .json file into Spark. I combined the reviews .json file with the businesses .json file (which contains business categories), and filtered by restaurant, in order to limit the scope of my analysis. I filtered by one and five-star reviews, in order to have greater contrast in my labels. Because there are many more five-star than one-star reviews, I limited the number of five-star reviews to the number of one-star reviews in order to avoid class imbalance. 

Next, I built my NLP preprocessing pipeline using Spark NLP, which involved tokenization, normalization, lemmatization, stopword removal, and bigram generation. I created a term frequency-inverse document frequency (tf-idf) vector upon which I would build my models.

Modeling started out with an 80-20 train-test split. The first model I attempted was a Latent Dirichlet allocation (LDA) model using Spark MLlib. However, this model was not very successful because the topics it generated referred more to the general features of restaurants (Asian, upscale, etc.) rather than the features associated with one versus five-star reviews. Second, I attempted a logistic regression model. However, this model did not align with the purpose of the project, which was not to predict one versus five-star reviews based on review text, but to understand what makes a one versus five-star review. Finally, I tried a random forest model, which is what I am currently working on. This appears to be the most appropriate approach because you can extract feature importances that are relative to their efficacy in classifying one versus five-star reviews. 

I am still working on various challenges at all stages of this analytics pipeline. First, I am trying different approaches regarding using only unigrams, unigrams + bigrams, only bigrams, or larger n-grams. Using only unigrams tends to generate features that are too general and obvious such as “delicious,” while using bigrams or larger n-grams fails to capture important features due to the highly combinatorial nature of language. One way to avoid this is to use part of speech (POS)-based chunking to identify meaningful bigrams and reduce noise in the dataset, which I may have to eventually implement. Another challenge involves the minimum and maximum document frequency (minDF and maxDF) parameters of count vectorization (i.e., the minimum and maximum number of documents a token has to appear in in order to be considered). If the minDF is too low, features will be too esoteric, and if the max DF is too high, features will be too general. I am currently working on playing with these parameters to generate the most insightful and salient results possible.

Overall, there is still much work to do, but if successful, this model should output meaningful, insightful, and salient features that could aid restaurant owners in making changes to avoid getting one-star reviews and acquire more five-star reviews. 
