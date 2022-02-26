# Forecasting Carbon Emissions
***Using Timeseries Regression Techniques to Predict Greenhouse Gas Emissions***

## Background and Problem Statement
Global warming presents a major challenge to humanity, and in an effort to quantify and forecast it's impacts, greenhouse gas emmissions predictions would be a reuqired input.  Timeseries data were provided for monthly greenhouse gas emmissions from a vareity of fuel sources used for electric energy production in the United States from 1973 to 2016.  The fuel soureces include natural gas, coal, and petroleum among others.  The goal of this project focused on forecasting the carbon diaoxide (CO2) emmisions from electric energy production fueld by natural gas for 12 months beyond the provided data.

## Initial Processing
As the emissions data included a vareity of sources, the natural gas data needed to be extracted into its own data frame for cleaning and pre-processing.  Note, while the attached code will show that all emissions data for the various fuels were plotted, this report will only consider the natural gas emmissions data as they were only emissions this project was tasked with forecasting.  After dropping unecesasry columns, the time data was converted from an integer object to a proper date-time format.  This step revealed that the time data contained a null value between every twleve-month interval, which was quickly determined to coincide with a twelve-mnoth (yearly) emissions sum.  The data corresponding to these null time values were removed as they are not part of a proper timeseries and of no use in forcasting.

## Summary of Evaluation and Forecasting Process






## Models Run

## Performance Evaluations

## Conclusion


# Contact Info
- Email: <a href = "mailto: drossetti12@gmail.com" style="color: red">drossetti12@gmail.com</a>
- LinkedIn: [https://www.linkedin.com/in/daniel-r-10882139/](https://www.linkedin.com/in/daniel-r-10882139/)
- MIT ADSP Certificate:  [https://www.credential.net/0655323a-4a50-49d6-9dfc-a81e5e4b7ca8#gs.o5fack](https://www.credential.net/0655323a-4a50-49d6-9dfc-a81e5e4b7ca8#gs.o5fack)
- MIT ADSP ePortfolio: [https://eportfolio.greatlearning.in/daniel-rossetti](https://eportfolio.greatlearning.in/daniel-rossetti)
