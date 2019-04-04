# Sparkify

## Project Definition
### Project Overview
A common problem in service providing companies (e.g. Spotify) is the prediction of [churn](https://en.wikipedia.org/wiki/Customer_attrition). Churn is, simply put, the loss of clients and it is a very common KPI to track. Prediciting churn allows the companies to take action to avoid the loss of customers before it happens, helping to increase their customer base.

### Problem Statement

An example dataset of events will be used to create a model that will predict churn in users. Each entry in the data set is an event such as:
* Listening to a song.
* Adding a friend.
* Logging in.

The dataset provided is a small sample (128 MB) of a full dataset available in AWS (12 GB). In real world applications the size of the dataset can be much larger which motivates the use of Spark for data handling and model training.

Churn will be defined as __customer that has visited the cancellation confirmation page__.

The tasks of this project are:
1. Load large datasets into Spark and manipulate them using SparkSQL or Spark dataframes.
2. Using Spark dataframes engineeer features that will help in the prediction of churn.
3. Use Spark ML to build and tune machine learning models that will predict churn.

### Metrics
Since we are dealing with churn, our labels will be heavily skewed. The number of users that churn represent around 10 % of all our users. For such problems accuracy is not a good metric since the model "user never churns" will score 90%.

To be able to determine how good our model is we will need to look at precision, recall and F1 score:
* Precision: How many of all the predicted churns are actually correct.
* Recall: How many of the churned customers did the model predict.
* F1: Harmonised average of precision and recall.
As we can see these metrics revolve around the churned customers, either predicted or actual.
Precision will be more useful when Type I errors are costly and recall will be better in cases where TypeII errors are undesirable. In absence of more information about the measures that will be taken based on our model, a fair compromise can be found by using __F1__.

## Analysis
## Conclusion
