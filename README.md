# AnomalyDetectionMain

## Approach/Algorithm Description
I first loaded the dataset and merge the datasets together (separated by cpc and cpm). In addition, I also sorted the dataset based on the timestamp of the data. After I finished loading the dataset, I decided to first make a plot between the timestamp and the value to explore any relationship between them. I realized that the fluctuation of the value is based on the different times in a day. Thus, I decided to apply a time-series based approach to detect the anomaly in the dataset. In addition, I also figured out that most of the anomaly are those values which are very large. Therefore, I assume that after I generate a trend from the time series model, I would classify those data points which are really far away from the trend as anomalies.

Since I haven't used any time series models before, I dived into the research of this field. After comparing several models, I decided to utilize ARIMA, short for 'Auto Regrressive Integrated Moving Average'. This model explains a given time series based on its own past values such as its own lags as well as the lagged forecast errors. An advantage of this model is that any non-seasonal time series that exhibits patterns and is not a random white noise can be modeled with ARIMA models. Several parameters need to be considered for ARIMA. The first one is lag (or the number of previous time periods). Since I thought that there is a daily pattern in the 'value' data, and the data is hourly based, I decided to use 0 here. Another parameter that needs to be considered here is the number of lagged forecast errors. I decided to use 2 here to allow more tolerance in errors. After I decided the values that I wanted to have for parameters, I put them in a .yaml file for the ease of use.

After I built the model and the model returned the predicted values for each timestamp, I needed to determine the standard of an anomaly. To achieve this, I decided to set a threshold for the absolute value between the predicted and actual values. If the difference exceeds the threshold, then the data point will be considered as an anomaly. I viewed the distribution of residuals and set 2 for cpc and 5 for cpm. These thresholds were also put in the .yaml file.

The result of the model is barely satisfying. It could only detect those anomalies with large values. It cannot successfully capture anomalies with relatively small values. In addition, as a result, it will also tend to classfy normal data with large values as anomalies even it could correctly capture the trend in the time series. This is probably because the model is using the moving average as a standard and thus it cannot react to a surprising increase in the value.

To improve this model, I am considering using other standard for identifying anomalies if time permitted. For instance, instead of using a hard threshold, using a confidence interval for cutoff might be a better idea. In addition, I am also wondering whether there is any better time series model to capture the trend more correctly and precisely. Moreover, I am also wondering whether there is any good idea to resolve the issue of duplicated timestamps in the dataset.

Finally, a problem I encountered during coding was that I was unable to utilize my Python module even though I successfully imported it. I spent a lot of time figuring out the reason why but it was not successful. To ensure that I could finish the main tasks, I decided to first code in the Jupyter Notebook which is supposed to use the module. I think that all the things I need are already in the module. Thus, after my initial submission, I would still try to use the Anomaly Detection module which I created to finish the task.

## Confusion Matrices
### exchange-2_cpc_results.csv
                               Predicted
                                0     1 
                            0| 1623 | 0 |
                     Actual  |------|---|
                            1|  1   | 0 |

### exchange-3_cpc_results.csv
                               Predicted
                                0     1 
                            0| 1535 | 0 |
                     Actual  |------|---|
                            1|  0   | 0 |

### exchange-4_cpc_results.csv
                               Predicted
                                0     1 
                            0| 1640 | 0 |
                     Actual  |------|---|
                            1|  2   | 1 |

### exchange-2_cpm_results.csv
                               Predicted
                                0     1 
                            0| 1622 | 0 |
                     Actual  |------|---|
                            1|  2   | 0 |

### exchange-3_cpm_results.csv
                               Predicted
                                0     1 
                            0| 1537 | 0 |
                     Actual  |------|---|
                            1|  1   | 0 |

### exchange-4_cpm_results.csv
                               Predicted
                                0     1 
                            0| 1635 | 4 |
                     Actual  |------|---|
                            1|  1   | 3 |