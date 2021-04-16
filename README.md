# [Project 1: DATA ANALYSIS OF MOVIE RATINGS](https://github.com/fivethirtyeight/data/tree/master/fandango)


### Goal: Is there a conflict of interest for a website that both sells movie tickets and diplays review ratings? Does a website like Fandango artificially display higher review ratings to sell more movie tickets?


### Tasks Performed: 
- Imported the data and researched about the background of the data.
- Explored Fandango Displayed Scores versus True User Ratings
- Compared Fandango Ratings to Other Sites


![](https://raw.githubusercontent.com/syednuman42/Syed_Portfolio/main/images/hickey-datalab-fandango-2.png)



## Part I : Importing the DATA and understanding it's Background:


After carefully reading the article and understanding the objective of the project, the necessary libraries and the csv files which contain the ratings of the movies from various movie review platforms were imported in the jupyter notebook.

![](https://raw.githubusercontent.com/syednuman42/Syed_Portfolio/main/images/fandango_data.PNG)



Then an exploratory data analysis was done to determine the relationships between the features of the dataset. 

![](https://raw.githubusercontent.com/syednuman42/Syed_Portfolio/main/images/eda1.png)

![](https://raw.githubusercontent.com/syednuman42/Syed_Portfolio/main/images/eda2.png)


Data Cleaning was done to remove all the films that had zero votes.



## Part 2: Exploring Fandango's Displayed Scores versus True User Ratings:

In order to examine the article's insinuation that Fandango doesn’t round a rating down when we’d mathematically expect it, a KDE plot was created that displayed the distribution of ratings that are displayed (STARS) on the website versus the true ratings that were from the votes (RATING) of users. The distribution of the kde plot of STARS DISPLAYED vs RATINGS FROM USERS indicated the same.

![](https://raw.githubusercontent.com/syednuman42/Syed_Portfolio/main/images/download%20(2).png)


This discrepancy was then quantified and a count plot was created to display the number of times a certain difference occured

![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/difference%20in%20ratings.png?raw=true)

## Part III : Comparison of Fandango's ratings with the ratings on other sites:

Since we only want to compare moves that are in both DataFrames (The Fandango Table and the All Sites table), they were combined using an inner merge based on the FILM column.

![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/inner%20merged%20data.PNG?raw=true)

RT,Metacritic, and IMDB do not use a score between 0-5 stars like Fandango does. In order to do a fair comparison, the data was normalized to fall between 0-5 stars

![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/normalized%20data.PNG?raw=true)

We already know that fandango pushes displayed RATING higher than STARS, the next question was whether ratings themselves were higher than average?

To answer this, a KDE plot was created that displayed the distribution of ratings across all the platforms.

![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/kde%20of%20all%20sites.png?raw=true)

Clearly, Fandango has an uneven distribution. Moreover, RT Critics had the most uniform distribution.

Consequently, Fandango and RT Critics were compared in order to visualize the bias of fandango's ratings clearly:

![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/kde%20of%20RT%20and%20fandango.png?raw=true)

Furthermore, The ratings of the top 10 worst movies on fandango were compared with their ratings across all other platforms:

![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/worst%20movies.PNG?raw=true)


![](https://github.com/syednuman42/Syed_Portfolio/blob/main/images/kde%20of%20worst%20movies.png?raw=true)



### Conclusion : Fandango artificially displays higher ratings than warranted to boost ticket sales

The article used in the project: [](https://fivethirtyeight.com/features/fandango-movies-ratings/)



# Project 2: PREDICTING ROCK DENSITY FROM X-RAY SIGNALS

### The problem: A tunnel boring company uses X-rays signals in an attempt to know rock density, ideally this will allow them to switch out boring heads on their tunnel boring machine before having to mine through the rock! We have experimental results based on a lab test on a variety of rock samples. Based on a rebound X-ray signal we wish to estimate a rock density.

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/Tunnel%20Boring%20machine.jpeg?raw=true)

### Goal :  To build a generalized model which could then be used to accept an X-ray rebound signal and output an expected Rock Density using various Regression Models.

### Models explored:
1. Linear Regression
2. Polynomial Regression
3. KNN Regression
4. Decision Tree Regression
5. Support Vector Regression
6. Random Forest Regression
7. Boosted Tree Regression


### Data Exploration

The necessary libraries and the data was imported. A scatterplot was created in order to explore the DataFrame 

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/df.PNG?raw=true)

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/scatterplot.png?raw=true)


We can see that the signal goes from 0 to 100 while the range of density goes from 1.6 to 2.8.
Moreover, there is only one explanatory variable. This implies that we won't have to deal with scaling the features


Then, the data was separated into explanatory (X-ray signal) and dependant(Rock-Density) variables and a train test split was performed


## Part 1: Linear Regression:

An instance of linear regression model was created and the training data was fit over it

After fitting the training data, predictions were calculated and performance of the model was evaluated

A root mean squared error of 0.257 indicated that this model was off about 11% of the time since the mean values of dependant variable is 2.2.
The model seems satisfactory but if we plot out the predictions we can see that a linear model cannot fit to this data

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/regplot.PNG?raw=true)


It's essentially just guessing the average value!



## Part 2: Polynomial regression

Polynomial Regression of degree 2 is created and plotted over the given data.

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/degree%202%20plot.PNG?raw=true)

Since, there is a Bias-Variance tradeoff between low and high degree polynomials, a degree 4 polynomial would be the best fit. 

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/degree%204.PNG?raw=true)


A root mean squared error of 0.146 indicates that this model is a good fit.

But what if we wanted to predict Rock Density outside the given range? Then this model would not be ideal.
Therefore we must explore other Regression methods


## Part 3: KNN Regression:

A KNN Regression model is created and plotted with 3 different values of k (1,5,10) :

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/knn1.PNG?raw=true)

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/knn%20plot.png?raw=true)

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/knn3.png?raw=true)


for k =1, there is a lot of noise and as k increases we start getting a smoother curve.

As we increase k, the bias will increase and it will start flattening out 

## Part 4 : Decision Tree Regression:

Since we only have 1 feature we dont have to worry about the hyperparameters

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/dt%20plot.png?raw=true)


We can see that it is picking up a lot of noise from the data. Therefore, it may be overfitting!

## Part 5:  Support Vector Regression:

A grid search is performed with various values of C and gamma.

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/svm%20plot.png?raw=true)

We can see that a support vector regression is performing quite well!


## Part 6 : Random Forest Regression: 

A large value of number of estimators won't make a huge difference as we only have one feature.

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/rf%20plot.png?raw=true)


We can see that it has a similar behavior to a decision tree. That is, it's picking up a lot of noise but it is fitting to the general curve


## Part 7: Boosted Tree Regression:

### AdaBoost Tree Regression:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/ada%20plot.png?raw=true)

### GradientBoost Tree Regression:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/project2/images/gb%20plot.png?raw=true)


We can see that both of these meta-learning methods performed quite well as they didn't pick too much noise.


### Conclusion : Support Vector, Adaboost and GradientBoost Regression Methods are the most suitable for this task
