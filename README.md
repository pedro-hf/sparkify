# Sparkify

## Project Definition
### Project Overview
A common problem in service providing companies (e.g. Spotify) is the prediction of 
[churn](https://en.wikipedia.org/wiki/Customer_attrition). Churn is, simply put, the loss of clients and it is a very 
common KPI to track. Predicting churn allows the companies to take action to avoid the loss of customers before it happens, 
helping to increase their customer base.

### Problem Statement

An example dataset of events will be used to create a model that will predict churn in users. Each entry in the data set 
is an event such as:
* Listening to a song.
* Adding a friend.
* Logging in.

The dataset provided is a small sample (128 MB) of a full dataset available in AWS (12 GB). In real world applications 
the size of the dataset can be much larger which motivates the use of Spark for data handling and model training.

Churn will be defined as __customer that has visited the cancellation confirmation page__.

The tasks of this project are:
1. Load large datasets into Spark and manipulate them using SparkSQL or Spark dataframes.
2. Using Spark dataframes engineeer features that will help in the prediction of churn.
3. Use Spark ML to build and tune machine learning models that will predict churn.

### Metrics
Since we are dealing with churn, our labels will be heavily skewed. The number of users that churn represent around 10 % 
of all our users. For such problems accuracy is not a good metric since the model "user never churns" will score 90%.

To be able to determine how good our model is we will need to look at precision, recall and F1 score:
* Precision: How many of all the predicted churns are actually correct.
* Recall: How many of the churned customers did the model predict.
* F1: Harmonised average of precision and recall.
As we can see these metrics revolve around the churned customers, either predicted or actual.
Precision will be more useful when Type I errors are costly and recall will be better in cases where TypeII errors are 
undesirable. In absence of more information about the measures that will be taken based on our model, a fair compromise 
can be found by using __F1__.

## Analysis
### Data Exploration
Our data is a sample of an event log. In our 128 MB sample data we have 286500 events of 255 users with 18 features each. 
The data extends between October and November 2018.
The following features are present:

* __Artist__: Artist of the song being played, if the event does not concern listening to a song the field is null
* __auth (authentication)__: Whether the user Logged in or Logged out, cancelled or guest.
* __firstName__: First name of the user. Null if user has not logged in
* __gender__: Gender of the user (M/F), null if the user has not logged in
* __itemInSession__: Number of events so far in the session. Starts in 0
* __lastName__: Last name of the user. Null if user has not logged in
* __length__: Time in seconds that a song has been played. if the event does not concert listening to a song the field 
is null.
* __level__: Whether the user has a paid subscription of free. (paid/free)
* __location__: Location of the user
* __page__: Page where the event took place. The following pages exists in the dataset: Cancel, Submit Downgrade, 
Thumbs Down, Home, Downgrade, Roll Advert, Logout, Save Settings, Cancellation Confirmation, About, Submit Registration, 
Settings, Login, Register, Add to Playlist, Add Friend, NextSong, Thumbs Up, Help, Upgrade, Error, Submit Upgrade
* __registration__: Registration number associated with each user
* __sessionId__: Number of the session initiated
* __song__: Name of the song being played.
* __status__: http status code (200, 307, 404)
* __ts__: Timestamp in ms
* __userAgent__: Information about the device and browser accessing the data.
* __userId__: Id associated with each user

And here is an example of data:

|              artist|     auth|firstName|gender|itemInSession|lastName|   length|level|            location|
|--------------------|---------|---------|------|-------------|--------|---------|-----|--------------------|
|      Martha Tilston|Logged In|    Colin|     M|           50| Freeman|277.89016| paid|     Bakersfield, CA|
|    Five Iron Frenzy|Logged In|    Micah|     M|           79|    Long|236.09424| free|Boston-Cambridge-...|
|        Adam Lambert|Logged In|    Colin|     M|           51| Freeman| 282.8273| paid|     Bakersfield, CA|

|           page| registration|sessionId|                song|status|           ts|           userAgent|userId|
|---------------|-------------|---------|--------------------|------|-------------|--------------------|------|
|       NextSong|1538173362000|       29|           Rockpools|   200|1538352117000|Mozilla/5.0 (Wind...|    30|
|       NextSong|1538331630000|        8|              Canada|   200|1538352180000|"Mozilla/5.0 (Win...|     9|
|       NextSong|1538173362000|       29|   Time For Miracles|   200|1538352394000|Mozilla/5.0 (Wind...|    30|

There are no duplicate events in our data but there are many rows without a userId. This is because events before login 
are registered, it those cases the user is `''`. Some of those events could be assigned to a user based on the sessionId,
if a session only has one user then all events of that session must belong to that user. However, only 2.9 % of events have
missing userIds and many sessions have more than one user. There is no obvious reason to think that those events are needed
for the purpose of this project and they will therefore be excluded in the rest of this analysis, leaving us with 278154 events.

### Exploratory Visualization
The picture below shows categorical features with less that 25 categories.

![alt text](pictures/categorical_features_barplot.png)

Most of our events are paid (80%), there are more events by women than by men and most of the events (>80%) are listening to songs 
(page:NextSong, method:PUT and status:200)
## Conclusion
