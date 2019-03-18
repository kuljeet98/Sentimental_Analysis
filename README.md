# Sentimental_Analysis
The data set we'll be working with today is the Amazon Reviews on Unlocked_Mobile phones dataset.
In this i have aplied the bag of approach and counter vectorizationorst, worthless and junk to negative reviews. And words like excellent, loves, and amazing to positive reviews.
4
Next, let's look at a different approach, which allows us to rescale features called tf–idf.
5
Tf–idf, or Term frequency-inverse document frequency, allows us to weight terms based on how important they are to a document.
6
High weight is given to terms that appear often in a particular document, but don't appear often in the corpus. Features with low tf–idf are either commonly used across all documents or rarely used and only occur in long documents.
7
Features with high tf–idf are frequently used within specific documents, but rarely used across all documents.
8
Similar to how we used CountVectorizer, we'll instantiate the tf–idf vectorizer and fit it to our training data.
9
Because tf–idf vectorizer goes through the same initial process of tokenizing the document, we can expect it to return the same number of features.
10
However, let's take a look at a few tricks for reducing the number of features that might help improve our model's performance or reduce a refitting.
11
CountVectorizor and tf–idf Vectorizor both take an argument, mindf, which allows us to specify a minimum number of documents in which a token needs to appear to become part of the vocabulary.
12
This helps us remove some words that might appear in only a few and are unlikely to be useful predictors. For example, here we'll pass in min_df = 5, which will remove any words from our vocabulary that appear in fewer than five documents.
13
Looking at the length, we can see we've reduced the number of features by over 35,000 to just under 18,000 features. Next, when we transform our training data, fit our model, make predictions on the transform test data, and compute the AUC score, we can see we, again, get an AUC of about 0.927. No improvement in AUC score, but we were able to get the same score using far fewer features. Let's take a look at which features have the smallest and largest tf–idf.
14
List of features with the smallest tf–idf either commonly appeared across all reviews or only appeared rarely in very long reviews.
15
List of features with the largest tf–idf contains words which appeared frequently in a review, but did not appear commonly across all reviews.
16
Looking at the smallest and largest coefficients from our new model, we can again see which words our model has connected to negative and positive reviews.
17
One problem with our previous bag-of-words approach is word order is disregarded. So, not an issue, phone is working is seen the same as an issue, phone is not working.
18
Our current model sees both of these reviews as negative reviews.
19
One way we can add some context is by adding sequences of word features known as n-grams. For example, bigrams, which count pairs of adjacent words, could give us features such as is working versus not working. And trigrams, which give us triplets of adjacent words, could give us features such as not an issue.
20
To create these n-gram features, we'll pass in a tuple to the parameter ngram_range, where the values correspond to the minimum length and maximum lengths of sequences.
21
For example, if I pass in the tuple, 1, 2, CountVectorizer will create features using the individual words, as well as the bigrams.
22
Let's see what kind of AUC score we can achieve by adding bigrams to our model.
23
Keep in mind that, although n-grams can be powerful in capturing meaning, longer sequences can cause an explosion of the number of features.
24
Just by adding bigrams, the number of features we have has increased to almost 200,000. And after training our logistic regression model and our new features, looks like by adding bigrams, we were able to improve our AUC score to 0.967. If we take a look at what features our model connected with negative reviews, we can see that we now have bigrams such as no good and not happy,
25
while for positive reviews we have not bad and no problems.
26
If we again try to predict not an issue, phone is working, and an issue, phone is not working, we can see that our newest model now correctly identifies them as positive and negative reviews respectively.
27
The vectorizers we saw in this tutorial are very flexible and also support tasks such as removing stop words or limitization. So be sure to check the documentation for more info. As always, thanks for watching.
28

