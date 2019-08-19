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

We also use R2 (Co-efficient of determination) as another metric to evaluate our models. It is explain below
![](https://github.com/karthikcs/capstone-project-flight-delay/blob/master/images/rsquarecanva2.png)


In our project we are using both MSE and R2 as metrics to evaluate all our models


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

![](https://raw.githubusercontent.com/karthikcs/capstone-project-flight-delay/master/images/airline_delay_distr-2.png)

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
Here we are using the following models:
-Linear regression
-Polynomial regression
-Light GBM 
-Ridge regression

Here **Linear regression** is taken as benchmark model and later let us move to polynomial regression as it provides of the relationship between the dependent and independent variable. Polynomial basically fits a wide range of curvature as well. And the end we will use Light GBM algorithm as the data is non-linear. It is good to try out a tree based model.
We split our data to train test and apply the above algorithms and compare the evaluation metrics.
The regularization technique ridge is also implemented for comparison. We have also used Grid search technique to find best plynomial order n and alpha-coefficient of ridge regression.

### Benchmark
As it is regression problem it is best to consider simple linear regression as benchmark model and try to beat its MSE value with other improvised models.
The below is the value of the MSE for our benchmark model that was used:
**MSE for Linear Regression	30.43 (Benchmark)**

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
In this section, the process for which metrics, algorithms, and techniques that you implemented for the given data will need to be clearly documented. It should be abundantly clear how the implementation was carried out, and discussion should be made regarding any complications that occurred during this process. Questions to ask yourself when writing this section:
- _Is it made clear how the algorithms and techniques were implemented with the given datasets or input data?_
- _Were there any complications with the original metrics or techniques that required changing prior to acquiring a solution?_
- _Was there any part of the coding process (e.g., writing complicated functions) that should be documented?_

### Refinement
In this section, you will need to discuss the process of improvement you made upon the algorithms and techniques you used in your implementation. For example, adjusting parameters for certain models to acquire improved solutions would fall under the refinement category. Your initial and final solutions should be reported, as well as any significant intermediate results as necessary. Questions to ask yourself when writing this section:
- _Has an initial solution been found and clearly reported?_
- _Is the process of improvement clearly documented, such as what techniques were used?_
- _Are intermediate and final solutions clearly reported as the process is improved?_


## IV. Results
_(approx. 2-3 pages)_

### Model Evaluation and Validation
In this section, the final model and any supporting qualities should be evaluated in detail. It should be clear how the final model was derived and why this model was chosen. In addition, some type of analysis should be used to validate the robustness of this model and its solution, such as manipulating the input data or environment to see how the model’s solution is affected (this is called sensitivity analysis). Questions to ask yourself when writing this section:
- _Is the final model reasonable and aligning with solution expectations? Are the final parameters of the model appropriate?_
- _Has the final model been tested with various inputs to evaluate whether the model generalizes well to unseen data?_
- _Is the model robust enough for the problem? Do small perturbations (changes) in training data or the input space greatly affect the results?_
- _Can results found from the model be trusted?_

### Justification
In this section, your model’s final solution and its results should be compared to the benchmark you established earlier in the project using some type of statistical analysis. You should also justify whether these results and the solution are significant enough to have solved the problem posed in the project. Questions to ask yourself when writing this section:
- _Are the final results found stronger than the benchmark result reported earlier?_
- _Have you thoroughly analyzed and discussed the final solution?_
- _Is the final solution significant enough to have solved the problem?_


## V. Conclusion
_(approx. 1-2 pages)_

### Free-Form Visualization
In this section, you will need to provide some form of visualization that emphasizes an important quality about the project. It is much more free-form, but should reasonably support a significant result or characteristic about the problem that you want to discuss. Questions to ask yourself when writing this section:
- _Have you visualized a relevant or important quality about the problem, dataset, input data, or results?_
- _Is the visualization thoroughly analyzed and discussed?_
- _If a plot is provided, are the axes, title, and datum clearly defined?_

### Reflection
In this section, you will summarize the entire end-to-end problem solution and discuss one or two particular aspects of the project you found interesting or difficult. You are expected to reflect on the project as a whole to show that you have a firm understanding of the entire process employed in your work. Questions to ask yourself when writing this section:
- _Have you thoroughly summarized the entire process you used for this project?_
- _Were there any interesting aspects of the project?_
- _Were there any difficult aspects of the project?_
- _Does the final model and solution fit your expectations for the problem, and should it be used in a general setting to solve these types of problems?_

### Improvement
In this section, you will need to provide discussion as to how one aspect of the implementation you designed could be improved. As an example, consider ways your implementation can be made more general, and what would need to be modified. You do not need to make this improvement, but the potential solutions resulting from these changes are considered and compared/contrasted to your current solution. Questions to ask yourself when writing this section:
- _Are there further improvements that could be made on the algorithms or techniques you used in this project?_
- _Were there algorithms or techniques you researched that you did not know how to implement, but would consider using if you knew how?_
- _If you used your final solution as the new benchmark, do you think an even better solution exists?_

-----------

**Before submitting, ask yourself. . .**

- Does the project report you’ve written follow a well-organized structure similar to that of the project template?
- Is each section (particularly **Analysis** and **Methodology**) written in a clear, concise and specific fashion? Are there any ambiguous terms or phrases that need clarification?
- Would the intended audience of your project be able to understand your analysis, methods, and results?
- Have you properly proof-read your project report to assure there are minimal grammatical and spelling mistakes?
- Are all the resources used for this project correctly cited and referenced?
- Is the code that implements your solution easily readable and properly commented?
- Does the code execute without error and produce results similar to those reported?
