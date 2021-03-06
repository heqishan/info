---
title: Part 5-7 Summary, Conclusion and Discussion-DEC10
toc: true
date: 2018-07-26 08:30:20
---

所有的项目都要有总结，summary ，这个 summary 写的很好，有配图，


# 5 Summary

## 5.1 About the project

Our goal is to <b>use time and weather to predict energy demand of buildings based on historical data</b>. The model will produce accurate energy demand forecasts that will be useful in smart grid technology.  If Harvard University can predict the energy demand of all campus buildings, they will be able to optimize the operations of chillers, boilers and energy storage systems.

There are three types of energy consumption, electricity, chilled water and steam. Chilled water is for cooling and steam is for heating. Chilled water and steam are generated in center plants and are delivered to buildings, just like electricity.


<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/7m154leAki.jpg?imageslim">
</p>

Image source: http://www.compression.org/distributed-energy/district-energy-distributed-energy-deschematic-640w/

<span style="color:red;">可见，真的很缜密，图片的来源都进行了标注。</span>

We tried five Machine Learning methods. <b>(1) Linear Regression (LR) (2) Support Vector Regression (SVR) (3) Gaussian Process Regression (GP) (4) Random Forests (RF) and (5) K-Nearest Neighbours (KNN).</b>


## 5.2 Findings from Exploratory Analysis

我们看到的一些分析的结果：

### Electricity consumption pattern

* Electricity shows a strong periodical pattern. You can clearly see the difference between day and night, weekdays and weekends.

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/G28jc6mC1D.png?imageslim">
</p>

* It looks like during each semester, electricity use ramps up toward a peak at finals, perhaps representative of studying patterns. The students are working harder and harder toward finals. Then there is a dip after semesters end, inlcuding Christmas vacation. The electricity consumption is relatively low during January and summer terms, and spring break, when campus can be relatively empty. (Text partially contributed by Steven)

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/dHBlcBccaB.jpg?imageslim">
</p>

### Relationship between Energy Consumption and Features

* Electricity is not co-related with weather data (temperature). The idea of using weather information to predict electricity will NOT work. I think it mostly depends on time/occupancy. But we can still do some pattern exploration to figure out day/night, weekday/weekend, school day/holiday electricity consumption pattern. Actually, we should have noticed that from monthly data.


* Chilled water and steam are strongly co-related with temperature and humidity. Daily chilled water and steam consumption have a good linear relationship with cooling and heating degrees. Even simple linear regression might be already quite accurate. However, using with weather information to predict hourly chilled water and steam is not suffient.

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/KcI99dKi6c.jpg?imageslim">
</p>

## 5.3 Prediction Accuracy of Different Machine Learning Methods

厉害，对每个模型都给出了自己的分析：

### Linear Regression

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/5F8b6lG51L.png?imageslim">
</p>

Advantage of the method: Simple and fast.

Disadvantage of the method: Poor results for large data sets. For example, hourly prediction.

### Support Vector Regression

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/5d6507Cdbf.png?imageslim">
</p>

### Gaussian Process Regression

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/f691fEDJlj.png?imageslim">
</p>

### Random Forests

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/HD1KmIl8cg.png?imageslim">
</p>

### K-Nearest Neighbours

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/iCHD9ikG00.png?imageslim">
</p>

* Please note that in Random Forests and K-Nearest Neighbours methods, the training set is larger and the test set is smaller compared with those in other methods.


* For KNN and RF predictions, the accuracy might increase if the features are carefully selected for different type of energy. For example, steam has nothing to do with dehumidification and cooling degrees, which are features designed for chilled water. Moreover, pressure, solar radiation, wind direction and wind speed do not have an impact on energy use according to our exploratory analysis. It is meaningless to include them in the prediction.


* Due to the time limitation, we didn't perform hourly prediction for all the methods.

# 6 Conclusion

下面是各个模型的对比：真的很清楚，厉害了。


<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/gIiBGKJeee.png?imageslim">
</p>

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/gID65ad8aA.png?imageslim">
</p>

### Daily Consumption

#### Chilled Water and Steam Prediction

* According to exploratory analysis, Daily chilled water and steam consumption have a good linear relationship with cooling and heating degrees. Even simple linear regression could predict daily chilled water and steam consumption quite well.


* Gaussian Process Regression and Random Forests perform slightly better than other methods in daily chilled water prediction. However, Random Forests prediction used a larger training set and smaller test set. The training test ratio is 2.4 : 1 for Random Forests and 1.1 : 1 for other methods. Therefore, it is not a fair comparison. The accuracy of RF could be lower if using the same training and test set.

#### Daily Electricity

* Daily electricity is not correlated with weather. Occupancy/schedule/study pattern have a large impact on daily electricity.


* Gaussian Process Regression outperforms other methods.

### Hourly Consumption

* Hourly precition is much more difficult than daily prediction. First, the data sample is large. Therefore, it is very time-consuming to train a model, expecially for those computationally expensive methods. Second, the noise and variance in hourly consumption are much larger than daily.


* Gaussian Process Regression did a good job predicting the hourly energy demand.


* Linear Regression prediction used a larger training set and smaller test set. The training test ratio is 2.8 : 1 ~ 8.7 : 1 for Linear Regression and 1.1 : 1 for Gaussian Process Regression. Therefore, it is not a fair comparison. The accuracy of Linear Regression Prediction for hourly prediction could be lower if using the same training and test set.


* Due to the time limitation, we didn't try all the methods for hourly prediction.


### The winner is Gaussian Process Regression.

他们介绍了对这个项目而言最优的模型是什么，以及讨论为什么选择这个模型。

* It is no doubt that Gaussian Process Regression outperforms other methods even in an <b><i>unfair</b></i> comparison. However, this does not mean Gaussian Process Regression is superior to other methods in genral. The features in Gaussian Process Regression are different from other methods. Maybe it is because we choose the right features for Gaussian Process regression.

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/180725/L0E3Fl93CA.png?imageslim">
</p>

A sample image of Gaussian Process Regression prediction.

#7 Discussion



* We spent a lot of time cleaning the raw data. Therefore, we can only manage prediction for one building.


* If we had more time, we would like to optimize the set of features that should be included in the prediction. For example, we could include more features such as weather data of previous hour, or even previous two hours. This is because the cooling and heating process is dynamic and there might be some delay in systems' reponse to weather. We probably could also exclude some irralevant features to reduce the time cost of training a model.


* We shoudl have used the same training and test time scale. Please note that in daily prediction, for Random Forests and K-Nearest Neighbours methods, the training set is larger and the test set is smaller compared with those for other methods. In hourly prediction, the trainng to test ratio (number of training points divided by number of test points) of Linear Regression is much lower than Gaussian Process Regression. These are not <b><i>fair</b></i> comparison.


* We tried our best to explain everything in the notebooks. However, there is very limited time for this project and the some part of work is trivial and difficult to explain. If there is anything unclear to you, do not heasitate to send us an email. We are happy to explain further. Please forgive any typos.
