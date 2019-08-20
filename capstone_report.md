# Machine Learning Engineer Nanodegree 
## Predicting flight delays - Capstone Project Report
Karthik Sunil <br>
August 18th, 2019

## I. Definition


### Project Overview
In the US, there are around 44,000 flights per day on an average. More than 2.5 million passengers travel through domestic flights per day. Air travel has become very stable and matured over couple of decades. However, there are still a lot of cancellations and delays that cause passengers a huge discomfort as well as loss in the businesses. 

There are different type of people who use the commercial flights for travel- tourism, business trips or personal trips to work/home. Delays in the flights will have impact on:

- Connecting flights
- Missing the business meetings 
- Customer discomfort

In this era of information, almost everyone takes informed decisions in their personal and business activities. To make decisions on the flight booking and planning more informed, prediction of delays/cancellation plays a mojor role

The datasets considered for this analysis and model building is from Kaggle https://www.kaggle.com/usdot/flight-delays
This dataset by DOT of USA contains all the flight data during 2015 by 14 different airlines. It has around 5.8 million flight data with airlines, source and destination airport, scheduled and actual deprature, arrival times, taxi-out/in data etc.

This is perfect data for this data project 

### Problem Statement
Flight booking services today provide the data about the price comparisons, no. of hops, flight time etc, however, it will add to customer delight if those services can provide a hint about predicted delays. It becomes a great boon for travellers if they know the predicted delays of flights much in advance which helps them to plan ahead.

Predcition of delays will also help airlines to plan ahead to avoid them. Also Airlines can reengineer their operative processes/ procedures which can reduce the flight delays in future. 

Based on the historical data of flights, one can figure out what are all the data which affect the delay. A regression model can be built to predict such delays in the flight departures. 

### Metrics
As our problem is based on prediction of departure delays based on regression model. Large differences between actual and predictedvalues are punished in MSE . Therefore we have taken MSE as one of our evaluation metrics. As we have tried different models including benchmark model which is linear regression they all should have common evaluation metrics so that we can compare which is the better model. 

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/MSE2.png)

Where y is expected value and y hat is predicted value

In our project we use MSE as metrics to evaluate all our models


## II. Analysis
In this section let us explore and try understand the data. We also perform Exploratory Data Analysis using visual graphics to find probably features of the model. 

### Data Exploration
Each entry of the input file `flights.csv` file corresponds to a flight and we see that more than 5.8 million flights have been recorded in 2015. These flights are described according to 31 variables. Here are few variables that I explain:
Inputs from the file:
- **YEAR, MONTH, DAY, DAY_OF_WEEK**: dates of the flight <br/>
- **AIRLINE**: An identification number assigned by US DOT to identify a unique airline <br/>
- **ORIGIN_AIRPORT** and **DESTINATION_AIRPORT**: code attributed by IATA to identify the airports <br/>
- **SCHEDULED_DEPARTURE** and **SCHEDULED_ARRIVAL** : scheduled times of take-off and landing <br/> 
- **DEPARTURE_TIME** and **ARRIVAL_TIME**: real times at which take-off and landing took place <br/> 
- **DEPARTURE_DELAY** and **ARRIVAL_DELAY**: difference (in minutes) between planned and real times <br/> 
- **DISTANCE**: distance (in miles)  <br/>

We also see the filling factor. Almost 99% of data records are filled, so I decided to drop rest 1% records, instead of filling with some estimates. This will not affect much on model because we have very huge number of records

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/FillFactor99.PNG)

Another problem occured during visualiztion and model building was that due to huge number of records, the RAM of the kernel was overflowing and hence I decided to sample the data. Instead of taking ramdom sampling, I did some analysis. I plotted the graph to see how the delay is varied compared to different month of the year 2015. Following graph shows the variation. I decided to take 1 month from each quarter so that we have sample across the year. *Feb, Jun, Aug and Dec* months were considered for further analysis and model building, which contains around 1.8 million records

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/month_delay.png)

**Output Variable**
As we have already established, the main purpose of this project is to predict the flight delays. In the dataset we have two type of delays. Departure and Arrival. From the below grpahics we can see determine that Arival delay is almost always is less than that of Departure delay. The plausible explanation could be, Airlines would be adjusting other factors like speed of the flight to cover up the Arrival delay. So, there are more factors which might affect the model if we consider Arrival delays. In this project we have considered Departure delay as the Y variable

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/Arrival_departure.png)

**Statistical Data by Airlines** 

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/Delay_Airline.PNG)

At fist glance we can see that Southwest Airlines (WN) has highest number of flights, with a mean delay of 12.6 min. We can consider that as best Airlines. Spirit Airlines (NK) with 22.3 min as mean delay can be considered as worst Airlines.

### Exploratory Visualization
In this section let us try to visually see what are all the features that are affecting the ```DEPARTURE_DELAY``` 

**AIRLINE** Feature <br>

In the following images, we can see how Airlines have different delays

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/airline_delay_distr-1.png)



In the pie chart that gives the percentage of flights per airline, we see that there is some disparity between the Airlines. For exemple, *Southwest Airlines* accounts for ~22% of the flights. However, if we have a look at the second pie chart, we see that here, on the contrary, the differences among airlines are less pronounced. Excluding *Hawaiian Airlines* and *Alaska Airlines* that report extremely low mean delays, we obtain that a value of 13+or-9 minutes would correctly represent all mean delays.

Finally, the figure at the bottom provides a census of all the delays that were measured in the sample data. This representation gives a feeling on the dispersion of data and put in perspective the relative homogeneity that appeared in the second pie chart. Indeed, we see that while all mean delays are around 10 minutes, this low value is a consequence of the fact that a majority of flights take off on time. However, we see that occasionally, we can face really large delays that can reach a few tens of hours !

The large majority of short delays is explained in the picture below:

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/delay_dist.png)

This graph above shows a flight delays less than 15 minutes, those in the range 15 to 60 min and finally, the delays greater than 60 minutes. Hence, we see clearly independent of airlines, delays greater than 60 minutes account only for a few percentage. However, the proportion of delays in these three groups depends on the airline: as an exemple, in the case of 
*SkyWest Airlines*, the delays greater than 60 minutes are only lower by ~30% with respect to delays in the range 15 < t < 60 min. Things are better for *SoutWest Airlines*  since delays greater than 60 minutes are 4 times less frequent than delays in the mid range.

Overall, we can also see the delay status of all flights as shown in the table below:

|Delay|Percentage of flights|
|---|---|
|Ontime|  79.4%|
|Small delay|  13.8%|
|Large delay|  6.8%|


**Time of the day** Feature <br>
Let us find delay pattern by time of the day 

![](https://github.com/karthikcs/capstone-project-flight-delay/blob/master/images/download.png)

In the above scatter plot we can see all the flights represented with point. The X Axis is time of departure time represented a number sequential seconds and Y axis by Departure Delay. We can see that delay suddenly raises by 5 AM and divergence of delays keeps reducing as day goes on. In the graph below, we can see average delay keeps increasing as the day goes on

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/dep_hr.png)


**Airport** Feature <br>
In the next section, let us try to find impact of ```ORIGIN_AIRPORT``` on ```DEPARTURE_DELAY``` 
The following grpah shows the heatmap of delays vs airport

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/heatmap.png)

In the above graph, we can see relationship with Airport and Airlines. Darker the color higher the delay. We definitely know Airport impacts a lot on Departure Delay. 


### Algorithms and Techniques

Here we are using the following models. 
- Linear regression
- Polynomial regression
- Light GBM 
- Ridge regression

Here **Linear regression** is taken as benchmark model and later let us move to polynomial regression as it provides of the relationship between the dependent and independent variable. Polynomial basically fits a wide range of curvature as well. And the end we will use Light GBM algorithm as the data is non-linear. It is good to try out a tree based model.
We split our data to train test and apply the above algorithms and compare the evaluation metrics.
The regularization technique ridge is also implemented for comparison. We have also used Grid search technique to find best plynomial order n and alpha-coefficient of ridge regression.

### Benchmark
As it is regression problem it is best to consider simple linear regression as benchmark model and try to beat its MSE value with other improvised models.
The below is the value of the MSE for our benchmark model that was used:

> **MSE for Linear Regression	32.47 (Benchmark)**

Liner regression can be mathematically expressed as:
Y = mX+b where: 
Y = Target variable or dependent variable, in our case it is departure delay
X=x1, x2 .. xn = Independent variables (features) which will impact the Target variable. In our case, they are 
origin airport, airline, time of departure etc m = set of coefficients of different features b = Bias, it is also Y
interecept


## III. Methodology

### Data Preprocessing
We have few data processing steps such as changing the format of Scheduled departure & arrival and Actual departure & Arrival time to stnadard *DATETIME* formats.

Next we treat missing values by removing them as ~99% of our data is complete.
We also remove the cancelled flights as they can not add any values in predicting the flight delays

Out of 3 feature variables two are Categorical variables - ```ORIGIN_AIRPORT``` and ```AIRLINE```. We convert them into Label encoding so that we can build the models. The model might get biased based on label encoded value, so it is preferred to use *OneHot Encoding*, however, due to the huge number of airports, the number columns created by encoding was not been able to handle by the kernel. So it was decided to go ahead with **Label encoding** itself.

In modelling we split the data into train and test. Also we have considered cross validation and regularization

### Implementation

The implementation has been done using Python and Jupyter Notebook. The following figure provides the details steps taken for this project and also provides the respective Section Numbers in the Jupyter Notebook.

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/steps2.PNG)

### Refinement
We have started our solution with Linear Model. But in the EDA we have already seen that the data is not Linear.

* **Polynomial Model** : We built the polynomial model with higher order. We run a grid search with various order. We found best results were obtained with 4th order
* **Ridge Regression**: We also improve polynomial model with Regularization technique using Ridge regression. We provide alpha and poly order in the grid search and found the best alpha was actually zero. 
* **LightGBM**: As we already hypothesised, solution for this problem will be something based on Tree based algorithm. We used LightGBM. This has many hyperparameters. ```boosting_type``` and ```learning_rate``` are most impacting the results. After multiple trials, we found ```boosting_type = 'gbdt'```  and ```learning_rate = 0.08``` fitted the data with best possibility. 

We can declare LightGBM as best fitted model for this data


## IV. Results

### Model Evaluation and Validation

Following table gives the results of the various models built for this problem. It also provides the details of the hyperparameters used in obtaining those results

|Model|Parameters|MSE|Comments|
|---|---|---|---|
|Linear|N/A|32.47|Baseline|
|Polynomial|Degree=2|32.33| |
| |Degree=3|31.52| |
| |Degree=4|30.96|Best in Polynomial |
|Random Forest|Estimators=100|32.3| |
|Ridge|Alpha=0, Degree=4|31.5| |
|LightGBM|boosting=gbdt, learning rate=0.08|27.78| **Best Model**|


### Justification
It is very evident with the above table that the tree based LightGBM with gbdt boosting type is the model fitted for this problem. Various parameters were tried and tuned for algorithm. Grid Search was also performed to get the best hyperparameters. After all exercise LightGBM with following parameters is considered the best model



|Parameter|Value|
|---|---|
|learning_rate|0.08|
|boosting_type|'gbdt'|
|objective|'regression'|
|metric|'mse'|
|sub_feature|0.5|
|num_leaves|100|
|min_data|5|
|max_depth|100|

Following points justifies that this model is good enough for the problem at hand

* The results obtained from this algoritm is almost 15% better than the baseline model. 
* The model predicts the flight departure delay with margin of plus or minus 5 min, where the maximum delay could be any where upto 60 min
* Provided the variability with respect to Airport and Airlines as depicted in EDA, this model predicts much better reliabiltiy


## V. Conclusion

### Free-Form Visualization
In the below graph we can clearly visualize the variability of flight delays by Departure time and Airline 

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/airline_delay_distr-2.png)

We can also deduce that most of the flights are on time and only a few flights rearely gets delayed. We can also see certain Airlines like *American Airline* have delays all over. The delays are sparsed across the time, while other airline like *Vergin America* the delays are more consistent.

### Reflection
When I selected this problem, I was thinking this is pretty simple nut to crack, however as I started the acquainted with the data, I realize it is not quite easy. 

First of all, while learning different models, we have usually seen typical linear and balanced data. However the data we took here is actual flight data and found it is pretty diversed and scattered across. Also the data is not balanced, in the sense, we have seen ~80% of flights are ontime and ~14% is haveing small delay and only ~6% is having large delay. Because of this, it was very difficult find the proper features that describes the target variable.

Another interesting observation about this dataset was that the volume of data. 5.8 million data was so difficult to manage even in the Cloud based Kaggle kernels. Certain pre-processing steps took more than 20 min to complete. So, I had to save the pre-processed data and load in the next run to save time. 

I started with the linear model and kept trying different regression models. As per the reviewer's suggions, I leanrt about LightGBM and implemented that model as well. It turned out that was the best model for this problem

### Improvement
Definitely, we can improve the solution for this problem. Following are some of the improvement ideas:

* First of all, we have eliminated Cancelled flights, however in the improved model we can also predict the possibiltiy of cancellation
* We have removed all the flights which have delays more than 1 hour. We can build a 2 step soltution, first one, which classifies if the flight gets **cancelled**, **short_delay**, **large_delay** or **ontime**. Then in the second step, it can predict the actual delay using regression
* We can perform an ensemble of LightGBM and Ridge Regression models for better generalization 
* We can also explore more hyperparameter tuning for LightGBM to improve the model
* In this solution, we took only 4 months of data due to low power kernels, we can get more powerful kernel and consider all 12 months data

-----------


