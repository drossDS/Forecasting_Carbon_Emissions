[<< Back to Project Portfolio Website](https://drossds.github.io/Dan_Data_Science_Portfolio/)

# Forecasting Carbon Emissions
***Using Timeseries Regression Techniques to Predict Greenhouse Gas Emissions***

## Background and Problem Statement
Global warming presents a major challenge to humanity, and in an effort to quantify and forecast it's impacts, greenhouse gas emmissions predictions would be a reuqired input.  Timeseries data were provided for monthly greenhouse gas emmissions from a vareity of fuel sources used for electric energy production in the United States from 1973 to 2016.  The fuel soureces include natural gas, coal, and petroleum among others.  The goal of this project focused on forecasting the carbon diaoxide (CO2) emmisions from electric energy production fueld by natural gas for 12 months beyond the provided data.

## Initial Processing
As the emissions data included a vareity of sources, the natural gas data needed to be extracted into its own data frame for cleaning and pre-processing.  Note, while the attached code will show that all emissions data for the various fuels were plotted, this report will only consider the natural gas emmissions data as they were only emissions this project was tasked with forecasting.  After dropping unecesasry columns, the time data was converted from an integer object to a proper date-time format.  This step revealed that the time data contained a null value between every twleve-month interval, which was quickly determined to coincide with a twelve-month (yearly) emissions sum.  The data corresponding to these null time values were removed as they are not part of a proper timeseries and of no use in forcasting.  A plot of the cleaned natural gas emission sdata is shown below.

****** Insert image opf all emissions data

From the plot, a distinct upward trend in natural gas emmissions can be observed from around 1990 onward that is inconsistent with the previous 17 years of data.  It further appears that this trend is maintained through the end of the data.  It is unknown what caused this to occur, however, it can be speculated the data prior to this event would not be relevent to a forecasting model for a time period beyond the provided data.  Thus, the data used for model fiting and forecasting was taken from 1990 onward.

## Exploraoty Data Analaysis
 - NEED TO SHOW THE DECOMPOSITION AND CONCLUDE MAXIMUM PREIOD OF SEASONAOLITY AS 12 MONTHS from the data
 - ALSO INCLUDE STATIONAIRTY RESULTS HERE FOR EACH DATASET



## Summary of Evaluation and Forecasting Process

____ basic steps to the process:
* Selecting input data processing techniques (data transformation techniques)
* Running transformations through 4 non-seasonal model varieties, iterated for p and q
* Choosing seasonal models for each best dataset
* Creating hybrid parameter model and model building
* Exploring input data timespans against validation data for each model, selecting lowest error model 
* Creating official forecast using proper datasets.

### Training Data Processing
The general rule is that the training data used to fit regression models must be stationary for the model to make a proper prediction.  However, it was determined during initial investiagtions that the non-sationary data actually produced the lowest-error predictions.  From this, four input datasets were created with various combinations of transformations to be run through all models (explained in further detail below) so that the datasets could be compared and the best tranformation method(s) selected.
- ***Raw Data***:  Untransformed natural gas emissions data in its "raw" form after having been cleaned and formatted as described previously
- ***Log Data***:  The Raw Data with a log transformation, attmepting to achieve stationarity
- ***Shifted Data***:  Raw Data which has been "shifted" by taking the difference between successive data point; effectively, the derivative of the dataset
- ***Log Shifted Data***:  Log-transformed Raw Data which was then "shifted" as described above
Of the four datasets above, only "shifted" datasets acheived stationarity as determined by an Augmented Dickey-Fuller (ADF) test.  As stated previously, stationarity had a negative impact on the model performacne as will be explained in the next section.

### Fitting and Optimizing Non-Seasonal Regression Models
Four model varieties were created including:
- Autoregressive (AR) Models
- Moving Average (MA) Models
- Autoregressive Moving Average (ARMA) Models
- Autoregressive Integrating Moving Average (ARIMA) Models

Each of these four models were trained against all four datasets above creating a combination of 16 model-dataset combinations.  Additionally, the p and q hyperparameters present in each of these model combinations were swepth through the values 0 through 12 with p and q being equal in all cases to limit computation time by limiting total hyperparameter combinations to just 13.  Thus, each of the 16 model-dataset combinations was run with hyperparameters p and q set equal through the values 0 - 12 for a grand total of 208 individual model-dataset-hyperparameter combinations.   Each combination was fit to the input training datasets, and their predictions were compared against the actaul emissions from the training data time period (1990 onward), and Root Mean Squared Error (RMSE) values were calculated for each model, and are shown in hte dropdown below.  
From error outputs, two important conclusions can eb drawn:
- Both shifted datasets produced much higher-error predictions, and thus the non-stationary datasets were providing much better predictions
- The model with the lowest error was an ARIMA model accepting the log-transformed data ("Log Data") with p = q = 10

### Fitting and Optimizing Seasonal Models
Using the auto_arima function from pmdarima, Seasonal ARIMA (SARIMA) models were created and optimized within the function, and the best performing model was output (as evaluated aginst AIC values).  From the above conclusion around stationarity data, auto_arima was only run with the raw and log-transformed datasets.  Within this function, the starting and maximum hyperparaemeter values can be selected and the model will run with each hyperparameter combintaion within the starting to maximum ranges.  After some trail-and-error, it was determined that non-seasonal hyperparaemters p and q would be run between the values 0 and 4 as ranges larger than this were prohibatively time-intensive.  The best model parameters for each dataset were then output and the RMSE values of their predictions against the training data were calcualted in the dame fashion as the non-seasonal models.  In an attemtp to create an even more accurate model, a final model was then speficied combining the parameters from the best seasonal and non-seasonal models.  The RMSE values and parameters for all 4 models are shown in the table below with all non-seasonal (p, d, q) and seasonal (P, D, Q, M) hyperparameters listed:

| Model Name | Type | Input Dataset | *p* | *d* | *q* | *P* | *D* | *Q* | *M* | RMSE |
| ---------- | ---- | ------------- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-:  |
| ARIMA_logdata| ARIMA | Log Data   | 10  |  1  | 10  | N/A | N/A | N/A | N/A | 2.2299|
| SARIMA_data| SARIMA


The RMSE values for each combinations predictions aginst their training data

* Creating input datasets:  checking for stationarity and establishing transformations 
* Raw emmissions data were decomposed into trend, seasonal and residual componentns ... this wasn't used for anything, maybe to inform ACF / PACF
* Then model functions were made for AR, MA, ARMA, ARIMA, SARIMA
*   Models were iterative swept through p and q
* Inverse transformation functions for Log shift, shift only, log only
* Made single funciton to optimize funcitons by sweeping p and q values together and selecting which type of function it was (not including SARIMA)
  *  Then ran optimizers on all 4 datasets and calcaulated RMSE values to find best models
*  Used auto ARIMA for run a SARIMA model with optimization
*  Combined sarima model with p and q parameters
*  Selected the 4 best models of those run....
*  Created train and validation (test) datasets using the last year of the provided data (note that we say train , but it's actually validation)
  * did this for 1990 onward data
* Then tried fitting the models against different input datasets for different time periods
* Then created averges for everything (but ignored that)
* Plotted 3 best models below RMSE 2, shose lowest RMSE as best model
* Then finally fit the best model


![](Emissions_Images/Model_Optimization_Process_Small.png)



![](Emissions_Images/Model_Timespan_Evaluation_Small.png)


Visually, the data are clearly not stationary, and stationarity is generally required for data 
* examined data stationarity with moving averages and dickey fuller tests
* attempted to remove sataionarity with transformations
* 

****** Add a 2x2 sbplot of all dat transformations


## Models Run

## Performance Evaluations

## Conclusion




# Contact Info
- Email: <a href = "mailto: drossetti12@gmail.com" style="color: red">drossetti12@gmail.com</a>
- LinkedIn: [https://www.linkedin.com/in/daniel-r-10882139/](https://www.linkedin.com/in/daniel-r-10882139/)
- MIT ADSP Certificate:  [https://www.credential.net/0655323a-4a50-49d6-9dfc-a81e5e4b7ca8#gs.o5fack](https://www.credential.net/0655323a-4a50-49d6-9dfc-a81e5e4b7ca8#gs.o5fack)
- MIT ADSP ePortfolio: [https://eportfolio.greatlearning.in/daniel-rossetti](https://eportfolio.greatlearning.in/daniel-rossetti)

<br><br>
[<< Back to Project Portfolio Website](https://drossds.github.io/Dan_Data_Science_Portfolio/)
