# Sentimental_Analysis
The data set we'll be working with today is the Amazon Reviews on Unlocked_Mobile phones dataset.
Looking at the head of the dataframe, we can see we have the Product Name, Brand, Price, Rating, Review text and the number of people who found the review helpful.
For our purposes, we'll be focusing on the Rating and Reviews columns.
Let's start by cleaning up the dataframe a bit. First, we'll drop any rows with missing values.
0:48
Next, let's remove any ratings = 3, we'll assume these are neutral.
Finally, we'll create a new column that will serve as our target for our model, where any reviews that were rated more than 3 will be encoded as a 1, indicating it was positively rated. Otherwise, it'll be encoded as a 0, indicating it was not positively rated.
Looking at the mean of the positively rated column, we can see that we have imbalanced classes.
Now, let's put our data into training and test sets using the reviews and positively rated columns.
Looking at X_train, we can see we have a series of over 231,000 reviews or documents. We'll need to convert these into a numeric representation that scikit-learn can use. The bag-of-words approach is simple and commonly used way to represent text for use in machine learning, which ignores structure and only counts how often each word occurs. CountVectorizer allows us to use the bag-of-words approach by converting a collection of text documents into a matrix of token counts.
First, we instantiate the CountVectorizer and fit it to our training data.
Fitting the CountVectorizer consists of the tokenization of the trained data and building of the vocabulary.
Fitting the CountVectorizer tokenizes each document by finding all sequences of characters of at least two letters or numbers separated by word boundaries. Converts everything to lowercase and builds a vocabulary using these tokens.
We can get the vocabulary by using the get_feature_names method.
This vocabulary is built on any tokens that occurred in the training data.
Looking at every 2,000th feature, we can get a small sense of what the vocabulary looks like. We can see it looks pretty messy, including words with numbers as well as misspellings.
By checking the length of get_feature_names, we can see that we're working with over 53,000 features.
Next, we use the transform method to transform the documents in X_train to a document term matrix, giving us the bag-of-word representation of X_train.
This representation is stored in a SciPy sparse matrix, where each row corresponds to a document and each column a word from our training vocabulary.
The entries in this matrix are the number of times each word appears in each document.
Because the number of words in the vocabulary is so much larger than the number of words that might appear in a single review, most entries of this matrix are zero.
Now let's use this feature matrix X_ train_ vectorized to train our model. We'll use LogisticRegression, which works well for high dimensional sparse data.
Next, we'll make predictions using X_test and compute the area under the curve score. We'll transform X_test using our vectorizer that was fitted to the training data.
Note that any words in X_test that didn't appear in X_train will just be ignored. Looking at our AUC score, we see we achieve a score of about 0.927. Let's take a look at the coefficients from our model.
Sorting them and looking at the ten smallest and ten largest coefficients, we can see the model has connected words like worst, worthless and junk to negative reviews. And words like excellent, loves, and amazing to positive reviews.
Next, let's look at a different approach, which allows us to rescale features called tf–idf.
Tf–idf, or Term frequency-inverse document frequency, allows us to weight terms based on how important they are to a document.
High weight is given to terms that appear often in a particular document, but don't appear often in the corpus. Features with low tf–idf are either commonly used across all documents or rarely used and only occur in long documents.
Features with high tf–idf are frequently used within specific documents, but rarely used across all documents.
Similar to how we used CountVectorizer, we'll instantiate the tf–idf vectorizer and fit it to our training data.
Because tf–idf vectorizer goes through the same initial process of tokenizing the document, we can expect it to return the same number of features.
However, let's take a look at a few tricks for reducing the number of features that might help improve our model's performance or reduce a refitting.
CountVectorizor and tf–idf Vectorizor both take an argument, mindf, which allows us to specify a minimum number of documents in which a token needs to appear to become part of the vocabulary.
This helps us remove some words that might appear in only a few and are unlikely to be useful predictors. For example, here we'll pass in min_df = 5, which will remove any words from our vocabulary that appear in fewer than five documents.
Looking at the length, we can see we've reduced the number of features by over 35,000 to just under 18,000 features. Next, when we transform our training data, fit our model, make predictions on the transform test data, and compute the AUC score, we can see we, again, get an AUC of about 0.927. No improvement in AUC score, but we were able to get the same score using far fewer features. Let's take a look at which features have the smallest and largest tf–idf.
List of features with the smallest tf–idf either commonly appeared across all reviews or only appeared rarely in very long reviews.
List of features with the largest tf–idf contains words which appeared frequently in a review, but did not appear commonly across all reviews.
Looking at the smallest and largest coefficients from our new model, we can again see which words our model has connected to negative and positive reviews.
One problem with our previous bag-of-words approach is word order is disregarded. So, not an issue, phone is working is seen the same as an issue, phone is not working.
Our current model sees both of these reviews as negative reviews.
One way we can add some context is by adding sequences of word features known as n-grams. For example, bigrams, which count pairs of adjacent words, could give us features such as is working versus not working. And trigrams, which give us triplets of adjacent words, could give us features such as not an issue.
To create these n-gram features, we'll pass in a tuple to the parameter ngram_range, where the values correspond to the minimum length and maximum lengths of sequences.
For example, if I pass in the tuple, 1, 2, CountVectorizer will create features using the individual words, as well as the bigrams.
Let's see what kind of AUC score we can achieve by adding bigrams to our model.
Keep in mind that, although n-grams can be powerful in capturing meaning, longer sequences can cause an explosion of the number of features.
Just by adding bigrams, the number of features we have has increased to almost 200,000. And after training our logistic regression model and our new features, looks like by adding bigrams, we were able to improve our AUC score to 0.967. If we take a look at what features our model connected with negative reviews, we can see that we now have bigrams such as no good and not happy,
while for positive reviews we have not bad and no problems.
If we again try to predict not an issue, phone is working, and an issue, phone is not working, we can see that our newest model now correctly identifies them as positive and negative reviews respectively.
The vectorizers we saw in this tutorial are very flexible and also support tasks such as removing stop words or limitization. So be sure to check the documentation for more info. As always, thanks for watching.
