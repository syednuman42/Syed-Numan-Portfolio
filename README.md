# [Project 1: DATA ANALYSIS OF MOVIE RATINGS](https://github.com/fivethirtyeight/data/tree/master/fandango)


### Goal: Is there a conflict of interest for a website that both sells movie tickets and diplays review ratings? Does a website like Fandango artificially display higher review ratings to sell more movie tickets?


### Tasks Performed: 
- Imported the data and read about the Background of the Data 
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
