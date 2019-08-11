# Modeling and Predicting the Flight Delay

## Proposal for Capstone Project - Udacity Machine Learning Engineer Nanodegree

Karthik Sunil <br>
Aug 8th, 2019

---

### This document explains the background for this capstone project. 

- The source of dataset 
- Understanding the data and data cleaning
- Exploratory data analysis approach
- Different visualizations that can be used data exploration
- Feature exploration that can be used for ML modelling 
- Exploring different approaches 

### Domain Background

In the US, there are around 44,000 flights per day on an average. More than 2.5 million passengers travel through domestic flights per day. Air travel has become very stable and matured over couple of decades. However, there are still a lot of cancellations and delays that cause passengers a huge discomfort as well as loss in the businesses. 

There are different type of people who use the commercial flights for travel- tourists, business trips or personal trips to work/home. Delays in the flights will have impact on:

- Connecting flights
- Missing the customer meetings 
- Customer discomfort

In this era of information, almost everyone takes informed decisions in their personal and business activities. To make decisions on the flight booking and planning more informed, prediction of delays/cancellation plays a mojor role

### Problem Statement

Flight booking services today provide the data about the price comparisons, no. of hops, flight time etc, however, it will be add to customer delight if those services can provide a hint about predicted delays/cancellations.It becomes a great boon for travellers if they know the predicted delay/cancellation of flights much in advance which helps them to plan ahead.

Based on the historical data of flights, one can figure out what are all the parameters which affect the delay/cancellation. A regression model can be built to predict such delays in the flight. 

### Datasets and Inputs

The datasets considered for this analysis and model building is from Kaggle https://www.kaggle.com/usdot/flight-delays
This dataset by DOT of USA contains all the flight data during 2015 by 14 different airlines. It has around 5.8 million flight data with airlines, source and destination airport, scheduled and actual deprature and arrival times. 

This is perfect data for this data exploration and modeling. 


### Solution Statement

#### Some definitions:
- Departure time: It is when a plane leaves the gate
- Arrival time: It is when the plane pulls up to the gate
- Scheduled time: Planned time of either departure or arrival
- Actual time: Actual time when the flight has departed or arrived 
- Delay: Difference between planned time and actual time, for for departure or arrival
- Taxi-out: It is defined as the time spent by a flight between its actual off-block time and actual take-off time. 
- Taxi-in: It is defined as the time spent after flight has landed till it reaches the block for disembarkment.
- Air time: Period during which a particular aircraft remains airborne. Also called wheels-off to wheels-on time.

By looking above definitions, we observe many parameters with respect to times during a flight. Some of them are interdependent. For egs. the arrival time depends on departure time, taxi-out, taxi-in and air time. 

In this project, I look for predecting the Arrival delay. 

The best approach to predict the delay in the data is Regression. Following are some of the regression methods we can explore:

1. Linear Regression
2. Polunomial Regression
3. Regularization methods like Ridge or Lasso Regression

*Liner regression* can be mathematically expressed as: <br>
``` Y = m1x1 + m2x2 + m3x3 + .. + mnxn + b  ```  where: <br> 
Y = Target variable, in our case it is departure delay <br> 
x1, x2 .. xn = Independent variables (features) which will impact the Target variable. In our case, it can be source airport, airline, time of departure etc
a1, a2, .. an = Coefficients of different features
b = Bias, it is also Y interecept

Similarly in case of *Polynomial Regression* one or more of the features have the degree more than 1 in the equation.

In *Regularization methods*, coefficient values are penalized by adding them L1 Regularization or in their squares, L2 Regularization to the loss function

### Benchmark Model

As per Federal Aviation Administration (*FAA*), General Aviation departure delays are 15 min or less. That can be considered as baseline for delays. With respect to building the model, we can consider source airport and airline are two feature variables and build a *Linear Regression model*. That can be considered as our benchmark model. 


### Evaluation Metrics

Some of common metrics considered for measurement of the regression model would be:
1. [Mean Absolute Error (MAE)](https://en.wikipedia.org/wiki/Mean_absolute_error)
2. [MSE (Mean Squared Error)](https://en.wikipedia.org/wiki/Mean_squared_error)
3. [R2 Score (Coefficient of determination)](https://en.wikipedia.org/wiki/Coefficient_of_determination)

In this solution design, I propose to use the MSE as the preferred metric for evaluating the model

### Project Design
_(approx. 1 page)_

In this final section, summarize a theoretical workflow for approaching a solution given the problem. Provide thorough discussion for what strategies you may consider employing, what analysis of the data might be required before being used, or which algorithms will be considered for your implementation. The workflow and discussion that you provide should align with the qualities of the previous sections. Additionally, you are encouraged to include small visualizations, pseudocode, or diagrams to aid in describing the project design, but it is not required. The discussion should clearly outline your intended workflow of the capstone project.

### References:
- [Predicting Taxi-Out Time at Congested Airports with Optimization-Based Support Vector Regression Method](https://www.hindawi.com/journals/mpe/2018/7509508/)
- [Flight Delay Information - Air Traffic Control System Command Center](https://www.fly.faa.gov/flyfaa/usmap.jsp)

-----------

**Before submitting your proposal, ask yourself. . .**

- Does the proposal you have written follow a well-organized structure similar to that of the project template?
- Is each section (particularly **Solution Statement** and **Project Design**) written in a clear, concise and specific fashion? Are there any ambiguous terms or phrases that need clarification?
- Would the intended audience of your project be able to understand your proposal?
- Have you properly proofread your proposal to assure there are minimal grammatical and spelling mistakes?
- Are all the resources used for this project correctly cited and referenced?
