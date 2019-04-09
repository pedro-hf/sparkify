# README
## Installations

The following packages are needed to run the notebook:
* numpy 1.16.2
* python 3.7.2
* pandas 0.24.2
* numpy 1.16.2
* pyspark 2.4.0
* matplotlib 3.0.3
* ua-parser 0.8.0

The sample data `mini_sparkify_event_data.json` of 128MB is not provided in this repository but can be accessed through 
the Spark Project Workspace at [Udacity's](https://www.udacity.com/) Data Science Nanodegree.
However, the transformed data `transformed_user_data` which is used in the modeling and tuning section is available in 
this repository.

## Project Motivation
A common problem in service providing companies (e.g. Spotify or Netflix) is the prediction of [churn](https://en.wikipedia.org/wiki/Churn_rate).
Churn is, simply put, the loss of clients and it is a very common KPI to track. Predicting churn allows the companies to 
take action to avoid the loss of customers before it happens, helping to increase their customer base. Iy is a complex
problem that involves handling large amounts of data. This notebook sets out to demonstrate an approach to solving this
problem using Spark and a sample event dataset.

The tasks of this project are:
* Load large datasets into Spark and manipulate them using SparkSQL or Spark dataframes.
* Using Spark dataframes, engineer features that will help in the prediction of churn.
* Use Spark ML to build and tune machine learning models that will predict churn

At the end of this work the reader should have a better understanding of the problems faced when dealing with churn prediction.
[This](https://medium.com/@pedro.hf86/sparkify-94376ad86a28?source=friends_link&sk=6ce2b604381dc512b3d4b749120da717) medium article gathers the results of the project.

## Repository Contents:
This repository contains:
* A jupyter notebook (`Sparkify.ipynb`)containing the main core of work of this project.
* A README file  (`README.md`) describing the project purpose and how to interact with it.
* A parquet file (`transformed_user_data`) containing the transformed data after the ETL process.
* a picture folder with the analysis results of the jupyter notebook.

To run this project locally you'll need to download the repository to your machine and obtain the `mini_sparkify_event_data.json` file. 
This can be obtained at Udacity's DataScience Nanodegree. You can also use other sample datasets with the same schema as the one presented in the notebook. This work was performed on a sample 
dataset of 128MB. It is still posible to run the modeling and tuning section (5) of the notebook without the `mini_sparkify_event_data.json`
file, since a transformed data set is provided with the repository.

## Notebook structure:

1. Import modules and create spark session
2. Load and Clean Dataset
3. Exploratory Data Analysis
    1. Explore Data
    2. User behaviour
    3. Define Churn
4. Feature Engineering
    1. Event-based features
    2. User-based features
    3. Save transformed data
5. Modeling
    1. Load and split data
    2. Define Evaluators
    3. Training and tuning
        * Logistic Regression Classifier
        * Random Forest Classifier
        * Multilayer Perceptron Classifier

