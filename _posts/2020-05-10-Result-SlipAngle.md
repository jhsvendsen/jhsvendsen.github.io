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

The dataset is only around 100.000 datapoints.

Manipulation done to dataset:
```python
# Removing NaN
dataset = dataset.dropna()

# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.1, random_state = 0)

# Feature Scaling
from sklearn.preprocessing import StandardScaler
sc_x = StandardScaler()
X_train = sc_x.fit_transform(X_train)
X_test = sc_x.transform(X_test)
```

## Program used
I chose to do this project in Python as the ML community and tools are fantastic and 
with a big community then it is only getting better by the day. I used specifically the 
package called Keras, with a TensorFlow background. This toolbox provides the engineer
with many toolboxes and data analysis tools to help make the correct decisions.

By using the keras toolbox, then installing the GPU accelerated version I am able
to use my 1050ti in training, which makes for greatly improved times.

Packages used:
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import keras
import sklearn
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.callbacks import History 
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.model_selection import GridSearchCV
from keras.wrappers.scikit_learn import KerasRegressor
```

## ANN Model
```python
regressor = Sequential()

# Adding the input layer and the first hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu', 
                    input_dim = 50))
regressor.add(Dropout(rate = 0.2))

# Adding the second hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu'))
regressor.add(Dropout(rate = 0.2))

# Adding the third hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu'))
regressor.add(Dropout(rate = 0.2))

# Adding the fourth hidden layer
regressor.add(Dense(units = 80, 
                    kernel_initializer = 'uniform', 
                    activation = 'relu'))
regressor.add(Dropout(rate = 0.2))


# Adding the output layer
regressor.add(Dense(units = 1))

# Compiling the ANN
regressor.compile(optimizer = 'adam', 
                  loss = 'mean_squared_error',
                  metrics=['mean_squared_error',
                           'logcosh'])


# Fitting the ANN to the Training set
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

Results of training - loss == MSE

{% include figure.html path="projects/annloss.png" alt="Ann Loss vs Epochs" %}

On the prediction vs test results we get a more visual picture for us to understand
the model. The model is quite good at predicting the slip angles, when slip angles
are almost 0. At very small slip angles we can see that the results are 
not quite as precise and that the model fluctuates more. 
Looking back at the loss curve we can see that the line is not a flat line, but
that it fluctuates, which could also tell us that it is having a hard time 
finding correlation between the dependent and independent variables, which
judging from the results below is an issue around 0 degree slip angle.

Random data picked out from the larger test set.

{% include figure.html path="projects/slip1.png" alt="Prediction Results 1" %}

{% include figure.html path="projects/slip2.png" alt="Prediction Results 2" %}

Overall the results are showing good predictions especially when one thinks of the
small amount of data that is used for the model.

Plotting code:
```python
# Values against each other
from matplotlib.pyplot import figure
fig = figure(figsize=(30,20))
plt.plot(y_pred)
plt.plot(y_test)
plt.title('Pred vs Test')
plt.ylabel('Slip angle')
plt.xlabel('Sample nr')
plt.legend(['Pred', 'Test'], loc='upper left')
plt.show()

# summarize history for loss
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'val'], loc='upper left')
plt.show()
``` 

# Conclusion
The work has shown that neural networks has potential to aid engineer in 
modelling tire dynamics, even with very little data (20 laps).

The model is able to quite accuratly predict the slip angles of the Lotus
R125 from Assetto Corsa. But in Assetto Corsa it seems that the tire model
is not very complex based on the low amount of layers needed to model the tire.
It would be interesting to put this model to the test with real life data,
as I imagine it will be much harder. But the results from this small experiment
done in my spare time, give me confidence that it will be possible although 
more complex networks might be needed.

# Future Work
Recurrent Neural Networks have great potential to improve the performance of the 
prediction, but the run times can be expected to be much great.