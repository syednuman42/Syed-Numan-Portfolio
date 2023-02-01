<html>
  <head>
  </head>
  <body onload="init();">
    <h1>Aadhaar Redaction / Selfie Recapture Detection Live Demo</h1>
   Click on the Start WebCam, then proceed to take photo.
     <p>
    <button onclick="startWebcam();">Start WebCam</button>
    <button onclick="snapshot2();">Selfie Recapture Detection</button> 
    <button onclick="snapshot();">Aadhaar Masking</button>      
    </p>
    
    <video onclick="snapshot(this);" width=300 height=300 id="video" controls autoplay></video>

  
   <pre>              Input Frame                              Detection Output </pre>

<canvas  id="myCanvas" width="300" height="300"></canvas>  
<img id="img" src="" height=300 width=300 />

  </body>
  <script>

      //--------------------
      // GET USER MEDIA CODE
      //--------------------
          navigator.getUserMedia = ( navigator.getUserMedia ||
                             navigator.webkitGetUserMedia ||
                             navigator.mozGetUserMedia ||
                             navigator.msGetUserMedia);

      var video;
      var webcamStream;
            
      function startWebcam() {
        if (navigator.getUserMedia) {
           navigator.getUserMedia (

              // constraints
              {audio: false,
               video: true
                 
              },

              // successCallback
              function(localMediaStream) {
                  video = document.querySelector('video');
                 video.srcObject=localMediaStream;
                 webcamStream = localMediaStream;
              },

              // errorCallback
              function(err) {
                 console.log("The following error occured: " + err);
              }
           );
        } else {
           console.log("getUserMedia not supported");
        }  
      }
            
      function stopWebcam() {
          webcamStream.stop();
      }
      //---------------------
      // TAKE A SNAPSHOT CODE
      //---------------------
      var canvas, ctx;

      function init() {
        // Get the canvas and obtain a context for
        // drawing in it
        canvas = document.getElementById("myCanvas");
        ctx = canvas.getContext('2d');
      }

      async function snapshot() {
         // Draws current image from the video element into the canvas
        canvas.getContext('2d').drawImage(video, 0, 0, 300,300);   
        img = canvas.toDataURL("image/jpeg").split(';base64,')[1];
        // console.log(img);
        datatosend = {'project':1,'byte_image':img};
        let result = await fetch("https://t79bfastr5.execute-api.ap-south-1.amazonaws.com/default", {                      
            method: "post",  
             headers: {
                'content-type': 'application/json'},
            body: JSON.stringify(datatosend)
             })
        .then(response=>response.json())

document.getElementById("img").src = result
      }
async function snapshot2() {
         // Draws current image from the video element into the canvas
        canvas.getContext('2d').drawImage(video, 0, 0, 300,300);   
        img = canvas.toDataURL("image/jpeg").split(';base64,')[1];
        // console.log(img);
        datatosend = {'project':2,'byte_image':img};
        let result = await fetch("https://t79bfastr5.execute-api.ap-south-1.amazonaws.com/default/portfolio", {                      
            method: "post",  
             headers: {
                'content-type': 'application/json'},
            body: JSON.stringify(datatosend)
             })
        .then(response=>response.json())

document.getElementById("img").src = result['body']
      }



  </script>
</html>



# MOVIE RATINGS ANALYSIS


### Goal: CLASSIFY STOCK DOCUMENTS CONTAINING AADHAAR NUMBER AND MASK THE FIRST 8 DIGITS

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





# Project 3: CHURN ANALYSIS AND CLASSIFICATION

### Background of the data: The data set includes information about
- Customers who left within the last month – the column is called Churn
- Services that each customer has signed up for – phone, multiple lines, internet, online security, online backup, device protection, tech support, and streaming TV and movies
- Customer account information – how long they’ve been a customer, contract, payment method, paperless billing, monthly charges, and total charges
- Demographic info about customers – gender, age range, and if they have partners and dependents

### Objective: 
- To analyze the characteristics of the customers that discontinued a service (churned). 
- To draw insights from the data in order to reduce the churn rate.
- Train a classification model to predict whether or not a customer will churn

## Part 1: Data Exploration

After importing the necessary libraries and the data, .info() method is called to explore the datatype of features

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/df.head.PNG?raw=true)

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/dfinfo.PNG?raw=true)

There are 7032 rows of data with 21 columns

We can see that there are a lot of categorical features so we will later have to create dummy variables for them.

The statistical summary of numerical data :

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/dfdescribe.PNG?raw=true)

We can confirm that there is no missing data by checking for null values


Checking the balance of the class label(Churn) with a countplot:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/churn%20countplot.png?raw=true)


The class label looks slightly imbalanced. However, there are quite a few instances of both the categories.



The feature - 'Contract type' has 3 categories: month-to-month, one year and two year contract type

We wish to explore the distribution of TotalCharges per Contract type and separate them based on whether they churned or not

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/tenure%20cohort%20vs%20contract.png?raw=true)


We can see that for contract type month to month, the distribution of customers who have churned is similar to that of those that did not churn. One possible reason for this could be that it is likely that **a customer that chooses month to month service does not plan to stay for a long period of time**.

What's interesting is that for the customers who have churned in the one/two year contract type the total charge's median is more than that of the people who did not churn.
This implies that **one/two year contract customers are more likely to churn if they had more total charge**

Assuming we cannot do much about the customers in the month-to-month category, our main objective now is to reduce churn rate for one and two year contract.
To do this we have to look deeper into the data to find out how to lower the charges for one/two year contract

The correlation between the features and the target label(churn):

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/feature%20correlation.png?raw=true)



## PART 2: CHURN ANALYSIS:

Exploring the count of customers with different tenure with a histogram:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/tenure%20countplot.png?raw=true)

We can see that there are a lot of customers with a small tenure i.e. customers who have just started and are maybe on a month to month contract

We can also see that there are a lot of customers with a high tenure value. These are the customers who have been using the service for a long period of time and will most likely continue to use it.

Moreover, there are spikes around 12,24,48 months mark. It is likely that these customers had a one/two year contract who churned after their contract was over.

Comparing tenure of customers based on contract and churn where columns represent contract type and rows represent churn

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/tenure%20displot.png?raw=true)

We can see that customers with one/two year contract type are less likely to churn than the month-to-month contract type

An interesting thing here is that there are a lot of customers with a high tenure yet they are still on month-to-month contract type. We know that a month-to-month contract type customer is more likely to churn. Therefore, if we can switch these high tenure month-to-month contract type customer to one/two year contract then maybe we could reduce their churn rate. This could be done by offering some discount on yearly plans.

Exploring Monthly charges vs Total Charges with a Scatterplot:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/total%20charges%20vs%20monthly%20charges%20scatterplot.png?raw=true)

This implies that a lot of people tend to churn if their monthly charges are higher. The company could then reduce monthly charges as from the company's perspective the goal is to maximize total charges.



## Part 3: Predictive Modelling:

1. Explanatory and dependant variables are separated. 
2. Dummies are created for the categorical variables.
3. The column 'customerID' is dropped since it has no pattern.
4. A train test split is performed using a test size of 10%

### Decision Tree Classifier

A decision tree model is initiated, fitted and trained. 

Then model predictions are calculated and test metrics are evaluated using a confusion matrix.

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/conf%20matrix%20dt.png?raw=true)

The confusion matrix shows that a decision tree classifier is performing much better on no churn than yes churn.
Almost 50% of the customers were wrongly classified to not churn.
Since our main objective is to correctly identify the people that did churn, the model is not perfomingly satisfactory!

Barplot showing the Importance of various features according to Decision Tree Classifier:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/feat%20imp%20dt.png?raw=true)

It was not clear from earlier data exploration that internet service fiber optics would be such an important feature!

### Random Forest Classifier

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/conf%20matrix%20rf.png?raw=true)


It looks like a random forest model with default values is performing worse than a decision tree classifier.
To improve this model we can do a grid search to tune the hyperparameters.

### Adaboost Classifier

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/conf%20matrix%20ada.png?raw=true)

AdaBoost is performing slightly better than a decision tree model


### Logistic Regression

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/conf%20matrix%20log.png?raw=true)


### Support Vector Classifier

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project%202/images/conf%20matrix%20svm.png?raw=true)


### Conclusion: Since all the methods are performing kind of similar. This might be the best possible outcome for this task. In addition to this, we could do some feature engineering or a grid search to improve the model.



# Project 4: UNSUPERVISED LEARNING USING KMeans CLUSTERING


### Goal: To gain insights into the similarites between countries and regions of the world by experimenting with different value of clusters.


## Part 1 : Exploring the data
Import the necessary libraries and the data

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/dfhead.PNG?raw=true)

Exploring the rows and columns of the data as well as the data types of the columns.

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/dfinfo.PNG)


There are 227 entries with 20 columns

Exploring the Population column with a histogram:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/population%20countplot.png?raw=true)

Due to a few large countries, the X axis is changed to only show the countries with less than 0.5 billion people

Now, let's explore GDP per capita and Regions using a bar chart:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/gdp%20vs%20region.png?raw=true)


It can seen that two regions (north america and western europe) in the world are way higher than the others

**The black bar represents standard deviation. In north america, the deviation is large because the United States is a huge outlier with a high GDP per capita**


Scatterplot showing the relationship between Phones per 1000 people and the GDP per Capita colored by Region:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/gdp%20vs%20phones.png?raw=true)


As gdp per capita grows, the phones per 1000 also grows

Scatterplot showing the relationship between GDP per Capita and Literacy colored  by Region:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/gdp%20vs%20literacy.png?raw=true)


**For low gdp, the literacy rate varies greatly but generally if the literacy rate is low then gdp will be less than 10000.
And when this 10000 gdp threshold is crossed, every country has a literacy rate of 90% or above**


A clustermap which auto performs hierarchal clustering of the correlations between columns:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/clustermap.png?raw=true)


## Part 2 : Data Preparation and Model Discovery 

Checking for missing data:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/isnull1.PNG?raw=true)

Most of the countries that have NaN for Agriculture are islands with the exception of Greenland and Western Sahara,

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/country%20is%20null1.PNG?raw=true)

So we will fill the missing value with 0 assuming that agriculture is not a signficant part of their economy

Checking for missing data again 

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/isnull2.PNG?raw=true)

**Industry and Service are also no longer missing values! This makes sense as most of the countries where agriculture was zero were tiny islands where industry and service are also not a major part of the economy**

**Climate and literacy % are missing for a few countries, but not the Region! Therefore, we can fill in the their missing values based on the mean value for its region.** 

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/groupby.PNG?raw=true)

Checking again for missing values, we can see that only a few missing values are left. For simplicity, we can drop these rows

It is now time to prepare the data for clustering. The Country column is still a unique identifier string, so it won't be useful for clustering. Dummy variables should be created for the categorical features.

Moreover, due to some measurements being in terms of percentages and other metrics being total counts (population), we should also scale this data first.

### Creating and Fitting Kmeans Model

Multiple KMeans models, testing from K=2-30 clusters are created and the Sum of Squared Distances(SSD) for each K value is stored, SSD values are plotted:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/ssd.png?raw=true)


We select a suitable value for k by looking at significant drop off in the value of SSD

A K value of 5 seems like a good choice to cluster countries based on the given features. As this is an unsupervised learning model, there isn't a 100% correct answer.


A bar plot showing the correlation of features with the cluster labels predicted by the Kmeans Model with 5 clusters:

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/corr%20of%20feat%20with%20label.png?raw=true)



The best way to interpret this model is through visualizing the clusters of countries on a map : 

![](https://github.com/syednuman42/Syed-Numan-Portfolio/blob/main/Project4/images/plotly.PNG?raw=true)



### Conclusion - Based on the features we were able to distinguish five main clusters of countries which are suprisingly good for an unsupervised model !


 

