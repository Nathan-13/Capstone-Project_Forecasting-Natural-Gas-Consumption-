# Capstone-Project_Predicting Natural Gas Daily Consumption
This is the final project repo for the Lighthouse Lab Data Science Capstone Project, which investigates various models to predict natural gas consumption in Saskatchewan in advance based on weather forecasts.

## Problem Definition and Motivation
Natural gas is pivotal in Saskatchewan's energy landscape, particularly for heating purposes. It stands out among fossil fuels for its efficiency, producing the least CO2 per 1 MJ of energy generated. As a transition fuel, it is expected to facilitate Saskatchewan's shift towards sustainable energy. 

Natural gas consumption in Saskatchewan is subject to various influences, including weather patterns and the economic growth of heavy industries like power plants, potash mines, agri-value facilities, and refineries. Precise forecasting of natural gas usage, based on historical demands and weather conditions, is vital for ensuring a steady and reliable energy supply. For transmission companies, accurate predictions are crucial to balancing supply and demand, averting shortages, and making strategic infrastructure investments. Weather conditions, particularly extreme temperatures, significantly impact natural gas consumption. Time series models capture these seasonal trends and patterns effectively, leading to more accurate forecasts. This, in turn, enhances operational efficiency, reduces costs, and improves customer service.

### Project Objective
•	To implement various forecasting models, both classical statistical and advanced neural network-based. This includes Prophet, LightGBM, SARIMAX, LSTM, and hybrid models.

•	Focus on implementing and understanding walk-forward validation and parameter optimization. It will evaluate the forecasting performance of the different models used.

•	The project aims to demonstrate that weather factors can be used to forecast the demand for natural gas in Saskatchewan accurately. Thus contributing to the field of energy forecasting.

These objectives will guide the project towards developing an effective and accurate forecasting system for natural gas consumption in Saskatchewan.

## Project Methodology
To accomplish the above project objective, we employed the following methodology: data collection/processing, model development, model evaluation, and model forecast. See the figure below to show the process flowchart. This flowchart provides a comprehensive overview of the steps in predicting future natural gas demand using historical data and machine learning models. It’s a great example of how data science can be applied to practical problems in the energy sector.
![Flowchart ](https://github.com/Nathan-13/Capstone-Project_Predicting-Natural-Gas-Consumption/assets/28906249/760d9ac7-b24f-4917-a4ed-3a4dce2a833c)

In the first stage, historical data on natural gas consumption was obtained from the official website of TransGas Ltd (www.transgas.com), Saskatchewan’s only natural gas transmission company, which has the exclusive right to transport natural gas within the province. The historical weather dataset was acquired from the Environment Canada Opendat website for 12 weather stations across Saskatchewan from November 1, 2013. Ten weather factors were selected, including the lowest relative humility, highest relative humility, heating degree days, cooling degree days, total precipitation, lowest temperature, average temperature, highest temperature, gust direction, and gust speed. An average of each weather factor from the 16 stations was obtained for a day to serve as an independent feature for the corresponding natural gas demand.

- **Data Pre-processing and Exploratory Data Analysis** 
This involves importing and understanding the dataset, cleaning the data, detecting and handling outliers, merging the weather dataset and natural gas load, analyzing and visualizing the relationships between the different variables, time series analysis, and preparing the data for modelling. 

- **Model Development** 
The combined dataset of daily natural gas consumption and weather factors for the past ten years is split into a learning set that spans 8.5 years (from November 1, 2013, to April 31, 2022) and a testing set that spans 1.5 years (from May 2022 to October 31, 2023). Furthermore, the learning dataset is subdivided into training sets (85%) and validation sets (15%) to train and validate the models. 
These models utilized include Prophet, LightGBM (Light Gradient Boosting Machine), SARIMAX (Seasonal Auto-Regressive Integrated Moving Average with eXogenous factors), LSTM (Long Short-Term Memory), and Hybrid LSTM-Prophet. In addition, a baseline model whose results can be used to assess our primary ones was selected as a Linear Regression model because of its simplicity and efficiency. The four models are developed in Python using numpy, pandas, stats models, Prophet, TensorFlow, and Scikit-Learn. 
The Time Series Split Cross-validation method, which splits data into multiple training and validation sets based on a specific time point, was implemented on the learning dataset to evaluate the performance of time series models. Hyperparameter tuning was introduced in the model training phase to determine the best parameters by either grid or random search techniques, after which evaluation is done on the validation set. The best parameters are now used to retrain the different models, after which evaluation is done on the validation set.

- **Model Evaluation**
In the model evaluation stage, the prediction performance of all the models is analyzed by comparing their test set predictions against observed test set data. The accuracy of these four models plus hybrids is compared using two statistical metrics: MSE, RMSE, MAE, MAPE, MDAPE, and R-squared. 

- **Predicting future Natural Gas demand**
In the model forecast stage, the model with the smallest RMSE and MAPE values is selected to forecast the natural gas demand for the next winter season in Saskatchewan.

## Project Results
### Exploratory Data Analysis
Two datasets are used in this project: the Natural gas consumption from the Transgas Daily Operations website and the weather factor data from 16 stations. See the "cleaning_EDA.ipynb" notebook for details.
- During the EDA step, there were no null values and duplicate rows in the Natural gas consumption dataset, and to forecast the daily consumption of natural gas in Saskatchewan using Time Series, the rest of the fields in Transgas daily operations data were removed, except "Date" and "Saskatchewan Deliveries."
- For the weather data from 16 stations across the province of Saskatchewan, missing values or NaN values were handled with zero imputation. This is because imputing annual seasonal mean or median values could significantly impact the performance of the model used to assess the influence of weather factors on natural gas demands; this decision is based on the assumption that zero would have a lesser impact on the model’s performance and there is the possibility of missing/empty values meaning absent of weather factors (e.g. Total Precipitation, Relative Humidity, etc will be zero in some period of the year). To reduce the complexity of the dataset in building the models, a daily average of 16 stations for each weather factor was obtained and then merged into the natural gas dataset.
- The Daily Natural Gas consumption ranges from 347 TJ/d to 1532 TJ/d, hinting at massive variation in consumption across winter and non-winter months. The heatmap shows a strong negative correlation between temperature factors (Lowest, Average, and highest) and the demand for natural gas - meaning the demand for natural gas rises with a decrease in weather temperature. While there is a less significant relationship between consumption and wind speed and direction. The heating degree days (HDD) factor was highly related to temperature factors, and the correlation coefficient to natural gas consumption is 0.86. See the figure below.
![Natural gas base   heating load](https://github.com/Nathan-13/Capstone-Project_Predicting-Natural-Gas-Consumption/assets/28906249/d32f21f7-8823-4b8b-bcec-588ec3bb8f68)


### Time Series Analysis
This analysis provides valuable insights into the characteristics of the time series and can guide the selection of appropriate forecasting models. See the "cleaning_EDA.ipynb" notebook for a detailed analysis.
![Time Series Decomp](https://github.com/Nathan-13/Capstone-Project_Predicting-Natural-Gas-Consumption/assets/28906249/85380854-7967-4ad0-8b07-6fac5deb2aeb)

1. The time series for Natural Gas Demand exhibits a **seasonal pattern**, influenced by factors recurring over a known period, such as the winter and non-winter seasons. There's no constant trend, suggesting a **non-linear trend**. The frequency and amplitude of the seasonal component do not change over time, suggesting an **additive seasonal model**. Comparing multiplicative and additive residuals, the latter is smaller, indicating that the additive model (Trend + Seasonality) fits the data more closely.

2. The **ADF test** statistic is -3.65, less than the critical values at the 1%, 5%, and 10% levels. This, along with a p-value of 0.0048, suggests that the time series is likely **stationary**. 

3. The **KPSS test** statistic is 1.907, greater than the critical values at the 1%, 2.5%, 5%, and 10% levels. This would typically suggest that the time series is **non-stationary**. However, the p-value of 0.01 contradicts this, indicating stationarity. This contradiction could suggest that the time series is **difference stationary**, meaning it can be made stationary by differencing the series a certain number of times.

4. The **Autocorrelation Function (ACF)** and the **Partial Autocorrelation Function (PACF)** for Natural Gas Demand show a significant spike at lag 0, indicating a strong autocorrelation with its immediate past value. Beyond lag 0, there is little to no correlation with past values at higher lags. This could be useful in forecasting future demand based on current and immediate past data.

5. Given that the time series is difference stationary, several models could be suitable for modelling and evaluation. 

This analysis provides valuable insights into the characteristics of the time series and can guide the selection of appropriate forecasting models.

### Time Series Models
#### Model Training and Validation
- Four-time series models (Prophet, SARIMAX, LightGBM, and LSTM), a hybrid model (LSTM-Prophet) and a Linear Regression, known as the baseline model, were trained and validated with the learning dataset. The evaluations of each model are done in separate notebooks to reduce tideous and long scrolling, for example training, validations and testing of the LSTM model, see the notebook 'model_lstm.ipynb'. The optimization of each model with hyperparameters improves the validation performances of the models.
- The LSTM models (single-layer and multi-layer, with and without optimization) outperform the Linear Regression model across all metrics. This could be due to the LSTM’s ability to capture temporal dependencies in the data, which a Linear Regression model might miata. counterparts. The Optimized LSTM-Prophet Hybrid model (see the notebook, 'model_hybrids.ipynb' for details) performs the best in terms of all the metrics used for evaluation. It has the lowest MSE, RMSE, MAPE, MAE, and MDAPE and the highest R-squared value, indicating the best fit to the data.
- Among the LightGBM model (model_lightgbm.ipynb), the cross-validation method resulted in the highest error Mean Squared Error (MSE) of 12428 and the lowest R-squared value of 0.72, indicating the lowest performance among the three. However, the hyperparameter-tuned Improved LightGBM model showed a slightly better performance in terms of Mean Absolute Percentage Error (MAPE), Mean Absolute Error (MAE), and Median Absolute Percentage Error (MDAPE) compared to the standard Cross-validation method and baseline model.
- In summary, the Optimized LSTM-Prophet Hybrid Model and the Optimized Single-layer LSTM appear to be the best models as they have the lowest error rates and the highest R-squared values. However, the baseline model, a Linear Regression model, and the rest of the models, which are LSTM models, also show improvements over the baseline model.

#### Model Testing and Evaluation
- The **LSTM-Prophet Model** performs the best with the lowest error metrics and the highest R-squared value of 0.98. The **LSTM Model** and the **SARIMAX Model** also show good performance with high R-squared values of 0.96 and 0.90, respectively. In contrast, the **Baseline Model** and the **LightGBM Model** have the highest error rates and lower R-squared values, indicating poorer performance. The **Prophet Model** also shows a commendable performance with an R-squared value of 0.90, indicating a good fit to the data. However, it's important to note that while these models perform well on this dataset, their performance may vary with different datasets. Therefore, it's crucial to choose the model based on the specific characteristics and requirements of the dataset.
- Based on the results, the LSTM-Prophet model emerged as the most accurate, with the lowest error metrics and the highest R-squared value. This suggests that the LSTM_Prophet model can effectively capture the complex patterns in the time series data, leading to more accurate forecasts. In summary, the LSTM-Prophet Model performs the best in prediction accuracy. The other models also show improvements over the Baseline Model, but not as much as the LSTM-Prophet Model. The testing metrics performance comparisons and the top 3 model predictions of the testing data set are shown below.
![Testing Performance metrics](https://github.com/Nathan-13/Capstone-Project_Predicting-Natural-Gas-Consumption/assets/28906249/be47e703-1769-4120-b182-f419c166d903)
![Testing Performance metrics2](https://github.com/Nathan-13/Capstone-Project_Predicting-Natural-Gas-Consumption/assets/28906249/dca31eed-0be8-40ef-9658-c423d197dcda)


![Top 3 model Testing predictions](https://github.com/Nathan-13/Capstone-Project_Predicting-Natural-Gas-Consumption/assets/28906249/d809ba9d-3592-46e9-a668-d5c41bb9dfb6)

#### Future Prediction
Due to time constraints and weather forecast data, the future prediction of Natural gas demand was not performed with the best model selected in the previous session.

## Conclusion
- This project aimed to develop an effective and accurate forecasting system for natural gas consumption in Saskatchewan, considering various influences such as weather patterns and economic growth of heavy industries. The project successfully implemented and evaluated various classical statistical and advanced neural network-based forecasting models, including Prophet, LightGBM, SARIMAX, LSTM, and hybrid models.

- The project demonstrated that weather factors can be used to forecast the demand for natural gas in Saskatchewan accurately. This contributes to the energy forecasting field and has practical implications for transmission companies in Saskatchewan. Accurate predictions can help these companies balance supply and demand, avert shortages, make strategic infrastructure investments, enhance operational efficiency, reduce costs, and improve customer service.

- In conclusion, this project has made significant strides toward facilitating Saskatchewan's shift towards sustainable energy by improving natural gas demand forecasting accuracy and efficiency. However, it's important to note that model performance can vary with changes in the data and regular retraining and validation of the models are recommended to maintain their predictive accuracy. Future work could explore other influencing factors and incorporate them into the models for even more accurate forecasts. Other advanced machine learning and deep learning models could also be explored and compared. The project has laid a solid foundation for future research and development.

## Challenges
Some of the challenges encountered during the project include computation power, pushing large files to Github, Data Quality and Availability, Model Complexity and Interpretability, and Feature Engineering and Selection.

## Future Work:
- Micro-level forecasting - Town Border Stations,
- Real-Time Forecasting Implementation, and
Integration with Natural Gas Market pricing and Economic growth.
