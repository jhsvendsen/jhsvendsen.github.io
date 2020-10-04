---
layout: post
title: Neural Network Results Slip Angle
date: '2020-10-05'
author: Johan Hentze Svendsen
tags:
- ANN
- Engineering
- Data
- Assetto Corsa
- Data analysis
thumbnail_path: projects/slipangle.jpg
---

## Introduction:  
I wanted to learn and investigate the possibility to use an ml model to approximate
the slip angle seen on a vehicle. A slip angle sensor is expensive and heavy, but 
has a great deal of advantages, such as being able to better approximate tire degradation
and performance. Modelling of tires has always been a major challenge and therefore makes
the possibility of using machine learning very appealing.

## Getting the data
Getting your hands on slip angle sensor data is difficult, so the method for this small
investigation was to use a sim racing game. Assetto Corsa is available on pc and has 
a fairly realistic vehicle and tire model. From this an app was created in which one 
could retract the data into a .csv file, which was then important into python.

App github page - https://github.com/ai-telemetry

The Assetto Corsa game has an "AI" feature in which the car drives it self around
the track. For a more consistent dataset then what I could provide with a controller
I chose to use this method and then have the car run 20 laps around Barcelona.

## Program used
I chose to do this project in Python as the ML community and tools are fantastic and 
with a big community then it is only getting better by the day. I used specifically the 
package called Keras, with a TensorFlow background. This toolbox provides the engineer
with many toolboxes and data analysis tools to help make the correct decisions.

## ANN Model
```python
regressor = Sequential()

Adding the input layer and the first hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu', 
                    input_dim = 50))
regressor.add(Dropout(rate = 0.2))

Adding the second hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu'))
regressor.add(Dropout(rate = 0.2))

Adding the third hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu'))
regressor.add(Dropout(rate = 0.2))

Adding the foruth hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu'))
regressor.add(Dropout(rate = 0.2))


Adding the output layer
regressor.add(Dense(units = 1))

Compiling the ANN
regressor.compile(optimizer = 'adam', 
                  loss = 'mean_squared_error',
                  metrics=['mean_squared_error',
                           'logcosh'])


Fitting the ANN to the Training set
history = regressor.fit(X_train, y_train, 
                        epochs = 200, batch_size = 200,
                         verbose=1, validation_split=0.33,
                          shuffle= True)
```    

The model was iterated through various parameters using both k-fold cross validation and
analyzing the output data, to make changes to the model.

## ANN Model Results

Below we can see the evolution of the loss (mean squared error), as a function of the 
number of epochs that the model has gone through. It can be seen that the model already
is converged around 150 epochs. It can also be seen that the model has an easier time
with predicting the results of the validation set, then on the training set. 

{% include figure.html path="projects/annloss.png" alt="Ann Loss vs Epochs" %}

On the prediction vs test results we get a more visual picture for us to understand
the model. The model is quite good at predicting the slip angles, when the car is not
driving straight, that is when slip angles are almost 0. At very small slip angles
we can see that the results are quite poor and that the model fluctuates more. 
Looking back at the loss curve we can see that the line is not a flat line, but
that it fluctuates, which could also tell us that it is having a hard time 
finding correlation between the dependent and independent variables, which
judging from the results below is an issue around 0 degree slip angle.

{% include figure.html path="projects/annpred.png" alt="Prediction Results" %}

# RNN


# Conclusion


# Future Work