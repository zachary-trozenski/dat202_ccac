## DAT-202: Data Analytics 2 Final Project

### Background

I'm very interested in weather and climate, so for my final project I wanted to try my hand at building two forecasting models on weather events. Digging around online I discovered there were several great resources with the most appealing being the National Climatic Data Center (NCDC) created by the National Oceanic and Atmospheric Administration (NOAA). Originally I had wanted to focus on severe weather events in my area, such as, floods, snowstorms, severe thunderstorms, etc. to work on building a model. I realized however that these weather events are too sporadic for novice modeling and wouldn't necessarily lend themselves well to forecasting. With that I pivoted to rainfall, specifically observed rainfall in Pittsburgh. I found that the NCDC publishes an hourly percipitation dataset for my region terminating in 2014. I decided to query data for a period of ten years and use that instead of severe weather. 

### About the data

The [data set](https://www.ncdc.noaa.gov/cdo-web/datasets#PRECIP_HLY) was queried from the NCDC website for hourly observed rainfall in Pittsburgh from 2004 to 2014 (January, 1 2014 is the final date for which data is available). The fields available are the date time (as the data is recorded hourly), the ID of the station where the observation was made, the station name in plaintext, and HPCP which is a measurement for the amount of rainfall observed within the hour in 1/100th's of an inch. In order to create a train and test set, I partitioned years 2004 through 2012 into the training set and kept 2013 - EOY as the test set.
Additional information can be found in the [data dictionary.](https://www1.ncdc.noaa.gov/pub/data/cdo/documentation/PRECIP_HLY_documentation.pdf)
#### Raw plotted training data (2004 - 2012)
![data_plot_1](./code/prophet_plot1.png)
#### Data plotted as a bar chart for better visability of seasonality
![data_plot_2](./code/prophet_plot2.png)


### Forecasting Models

Since the data is seasonal I wanted to use Prophet as the tool is able to handle seasonality out of the box with mininal data processing. While Prophet seemed like the obvious choice, I wanted to try working with and ARIMA model to try something a little more challenging. In the subsections below I'll discuss the models used, the work involved in building each indluding output graphs, and finally a brief conclusion on which model worked better for this data set.

#### Prophet

The Prophet model seemed to be the best fit for the data as it has the ability to handle seasonality. That being said weather is inheritely more unpredictable and more heavily impact by other factors than Prophet may be able to handle. 
The model started by importing all the libraries I would use in my code and loading the dataframe. Once the dataframe was initialized, I normalized the data by removed the data set's notation for NaN values. The final preperatory task was the subset the dataframe to only contain the Date and HPCP values with new column headers; "ds" and "y".
From there it was a simple as creating a model object by calling the Prophet function and fitting the model to the training data. The output of model returned a dataframe with predicted y-hat values. 
Here is a plot of the training data and the model's prediction:
![prophet_pred1](./code/prophet_pred_rain.png)

And here are the components of the model on the test set:
![prophet_pred2](./code/prophet_model_comp.png)

Finally I plotted the model's prediction with the full rainfall data set (left) as well as a snapshot of the real and predicted values for only the test set (right):
![prophet_pred_act](./code/prophet_pred_act.png)

And for a closer look, I split the number of observations into four roughly equally sized parts and graphed them for better visibility:
![prophet_pred_act2](./code/prophet_pred_act2.png)

To see all of my Prophet model code, please [visit this link in my repo.](https://github.com/zachary-trozenski/dat202_ccac/blob/master/final_project/code/Final_Project_Prophet_code.ipynb)

#### ARIMA


#### Conclusions


### References

Prophet Jupyter Notebook by C. Sheldon-Hess (Blackboard)

[Advanced Time Series Modeling (ARIMA) Models in Python](https://www.pluralsight.com/guides/advanced-time-series-modeling-(arima)-models-in-python)

[Using Python and Auto ARIMA to Forecast Seasonal Time Series](https://medium.com/@josemarcialportilla/using-python-and-auto-arima-to-forecast-seasonal-time-series-90877adff03c)

[Statsmodel Documentation](https://www.statsmodels.org)

[Python | ARIMA Model for Time Series Forecasting](https://www.geeksforgeeks.org/python-arima-model-for-time-series-forecasting/)

[How to use SARIMAX](https://www.kaggle.com/poiupoiu/how-to-use-sarimax)

[Seasonal ARIMA with Python](https://www.seanabu.com/2016/03/22/time-series-seasonal-ARIMA-model-in-python/)

[pmdarima Documentation](https://alkaline-ml.com/pmdarima/)

[Why I get the same predict value in Arima model?](https://stats.stackexchange.com/questions/333092/why-i-get-the-same-predict-value-in-arima-model)

[Machine Learning Mastery](https://machinelearningmastery.com/)

[ARMA out-of-sample prediction with statsmodels](https://stackoverflow.com/questions/18616588/arma-out-of-sample-prediction-with-statsmodels)

[Detecting Stationarity in time series data](https://towardsdatascience.com/detecting-stationarity-in-time-series-data-d29e0a21e638)

[Stationarity and Differencing](https://otexts.com/fpp2/stationarity.html)

[Prophet Documentation](https://facebook.github.io/prophet/docs/quick_start.html)

[Python: “Pandas data cast to numpy dtype of object. Check input data with np.asarray(data).”](https://stackoverflow.com/questions/46534512/python-pandas-data-cast-to-numpy-dtype-of-object-check-input-data-with-np-asa)

[Error: ValueWarning: A date index has been provided, but it has no associated frequency information and so will be ignored when e.g. forecasting](https://stackoverflow.com/questions/58510659/error-valuewarning-a-date-index-has-been-provided-but-it-has-no-associated-fr)

[Where is the documentation on Pandas 'Freq' tags? [closed]](https://stackoverflow.com/questions/35339139/where-is-the-documentation-on-pandas-freq-tags)

[Set pandas.tseries.index.DatetimeIndex.freq with inferred_freq](https://stackoverflow.com/questions/40222583/set-pandas-tseries-index-datetimeindex-freq-with-inferred-freq#40222783)
