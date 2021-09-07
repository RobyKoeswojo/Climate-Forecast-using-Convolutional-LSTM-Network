# Tokyo Climate Forecast using Convolutional LSTM Network

**Tags**: *time series*, *forecast*, *prediction*, *convolutional layer*, *recurrent neural network (RNN)*, *long short term memory (LSTM)*, *Tensorflow*, *Tensorflow Data*

One of the most interesting stories from the Tokyo Olympics is the number of media reporting how the heat affects the athletes. 
Many athletes fainted and some were forced to leave the competition in wheelchair due to the heat.
The quadrennial event was thus labeled as "the hottest games ever", and all of this is indeed caused by none other than the climate change.

After reading those news, an idea popped into my mind about modeling the climate forecast in Tokyo using neural network. 
The climate forecast exhibited here is a univariate time series forecasting that predicts solely based on the previous temperature information
(here, the humidity which also has an impact on the heat felt by the athletes is not considered).  

The neural network used is a convolutional LSTM network demonstrated in the [Sequences, Time Series and Prediction](https://www.coursera.org/learn/tensorflow-sequences-time-series-and-prediction) course in coursera, 
where it consists of a 1D-convolutional layer, followed by two layers of LSTMs, two layers of fully-connected layers and an output layer.

## Environment  
This notebook is run on google colab using ``Tensorflow 2.6.0``  

## Data  
The data used is the mean daily maximum temperature released every month by the Japan Meteorological Agency.  
The data is available here: https://www.data.jma.go.jp/obd/stats/etrn/view/monthly_s3_en.php?block_no=47662&view=2  
80% of the data points are used as the train set, whereas the remaining 20% are taken as the test set.

| ![Plot of the mean daily maximum temperature from Jan 1784- Dec 2020](https://github.com/RobyKoeswojo/Climate-Forecast-using-Convolutional-LSTM-Network/blob/main/notebookimage/TimeSeriesPlot.png?raw=true) |
|:--:| 
| Plot of the mean daily maximum temperature from Jan 1784- Dec 2020 |

## Training the network  
The convolutional LSTM network is trained for 100 epochs using Huber loss function optimized by stochastic gradient descent. 
The metrics chosen to measure the performance of the model is mean absolute error (MAE).  
A learning rate finder is applied beforehand to obtain the optimal learning rate.  

| ![Learning rate optimization plot shows that 1e-5 is the optimal learning rate value](https://github.com/RobyKoeswojo/Climate-Forecast-using-Convolutional-LSTM-Network/blob/main/notebookimage/BestLR.png?raw=true) |
|:--:| 
| Learning rate optimization plot shows that 1e-5 is the optimal learning rate value |

Using the optimal learning rate, the network is trained and its performance is evaluated on the test set.  

| ![The training loss plot over 100 epochs. The train loss value rapidly drops as the training starts.](https://github.com/RobyKoeswojo/Climate-Forecast-using-Convolutional-LSTM-Network/blob/main/notebookimage/LossPlot_baseline.png?raw=true) |
|:--:| 
| The training loss plot over 100 epochs. The train loss value rapidly drops as the training starts. |

## Result
The performance of the convolutional LSTM network is satisfactory with **train MAE score** of **1.489** and **test MAE score** of **1.386**.  
However, a hyperparameter tuning that optimizes the number of filters and kernels of the convolutional layer and the number of LSTM and dense units 
is done to see if better performance can be achieved.  
The outcome is a slightly better tuned network with **train MAE score** of **1.257** and **test MAE score** of **1.323**, and the comparison of the (baseline) network to the tuned network is as following: 

![Train and test MAE score of the baseline network vs tuned network](https://github.com/RobyKoeswojo/Climate-Forecast-using-Convolutional-LSTM-Network/blob/main/notebookimage/trainTestScore.JPG?raw=true)
|:--:| 
| Train and test MAE score of the baseline network vs tuned network |

## Forecasting 2021 temperature  
The tuned convolutional LSTM network tries to forecast the temperature from January - July 2021, in which their true values are already recorded in the JMA website.
As shown in below image, at some months, the error of the forecasted temperature is over than 2 degrees. However, averaging the difference (error) values from Jan - Jul,
the **average error value** is **1.24**, which is similar to the train and test MAE score of the network.

| ![Forecasting temperature from Jan - Jul 2021](https://github.com/RobyKoeswojo/Climate-Forecast-using-Convolutional-LSTM-Network/blob/main/notebookimage/forecast2021.JPG?raw=true) |
|:--:| 
| Tokyo's forecasted temperature from Jan - Jul 2021. The average difference (error) value is 1.24 |
