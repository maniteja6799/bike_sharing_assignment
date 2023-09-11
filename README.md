# Bike Sharing assignment

2 year dataset of a bike sharing platform is shared. This is an attempt to understand the data, create a linear regression model to determine the impact of measured metrics over the period. An estimate model is created at the end of this exercise to predict the number of rentals made on a certain date given the measured data. More details on the description of measured data is below.

## Table of Contents

- [General Information](#general-information)
- [Data characteristics](#dataset-characteristics)
- [Conclusions](#conclusions)
- [Technologies Used](#technologies-used)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)

## General Information

This project is to create a linear regression model to the provided bike sharing dataset to find out various parameters which could affect the rentals.

Following EDA and Data transformations applied before the model creation:

- Pair-plot on dependant and independent vars to identify their relationships
- Derived Metrics and Dropped columns
- Box plots on categorical variables
- Creating dummies for categorical vars
- Splitting data to train and test
  
Model creations first using stats models, In combination of model summary and VIF feature selection was done (more details in conclusions below). RFE technique also applied to randomly choose the features which also resulted creation of similar model.

## Dataset characteristics

day.csv have the following fields:

- instant: record index
- dteday : date
- season : season (1:spring, 2:summer, 3:fall, 4:winter)
- yr : year (0: 2018, 1:2019)
- mnth : month ( 1 to 12)
- holiday : weather day is a holiday or not (extracted from http://dchr.dc.gov/page/holiday-schedule)
- weekday : day of the week
- workingday : if day is neither weekend nor holiday is 1, otherwise is 0.
- weathersit :
  - 1: Clear, Few clouds, Partly cloudy, Partly cloudy
  - 2: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist
  - 3: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds
  - 4: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog
- temp : temperature in Celsius
- atemp: feeling temperature in Celsius
- hum: humidity
- windspeed: wind speed
- casual: count of casual users
- registered: count of registered users
- cnt: count of total rental bikes including both casual and registered

## Conclusions

### Insights from EDA

1. The weather situation value is never 4, It can indicate either missing data or the weather was never type 4 (Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog) in the recorded two years dataset.
2. Fall season seems to have the highest rentals across the season. Which also correlates to the corresponding months having higher rentals
3. There are more rentals recorded when the weather is clear
4. `temp` and `atemp` are highly correlated and have a good linear pattern with rentals.
5. There doesn't seem to be a linear relation in between humidiy, windspeed to the rentals recorded.

### Predicted models and dropped vars through development

- `casual` and `registered` columns just sum up to the outcome dependat variable `cnt`. They should ideally not be considered as independent variables because they are actually outcomes. As per the business statement they can't be used in the training data for X.
- With all the vars from training dataset, the training data r-squared came out to be 0.834 which is pretty good for the way how the model is explaining the variance in the outcome rentals. but we can clearly see some insignificant vars from the model summary. `atemp` has a p-value of 0.693 and a VIF of 365. It can be good first candidate to drop and retrain a model
- After removing `atemp` the r-squared value didn't change, but we can see the VIF for temp has reduced drastically. Next possible candidate is `mnth` with a p-value of 0.633 and VIF of 17.51
- After removing `mnth` the r-squared value didn't change either. Next possible candidate is `date` with a p-value of 0.538
- Since `temp` has a high VIF, trying out removing it too from the vars. This resulted in a lower r-squared. The higher VIF is most likely because of temperature being dependent on other vars like humidity, windspeed up to some extent. But they alone wouldn't be fully responsible for temp and thus the outcome rentals. So keeping all those is ok for achieving a higher r-squared on the model.
- [OLS Stats model + Manual feature selection] R-squared on the training data is `0.834` and on test data is `0.809`. This is a good close value with training data indicating the model can generalise for unseen data and not too overly fitting training data.
- [RFE + sklearn Linear Regression] RFE also produced a similar model like the stats model above with an R-squared on the training data is `0.827` and on test data is `0.806` . With similar confidence intervals as well.

## Technologies Used

- Python 3.10.9
- Jupyterlab 3.6.3
- numpy 1.23.5
- pandas 1.5.3
- matplotlib 3.7.0
- seaborn 0.12.2

<!-- As the libraries versions keep on changing, it is recommended to mention the version of library used in this project -->

## Acknowledgements

Reference dataset:
[1] Fanaee-T, Hadi, and Gama, Joao, "Event labeling combining ensemble detectors and background knowledge", Progress in Artificial Intelligence (2013): pp. 1-15, Springer Berlin Heidelberg, doi:10.1007/s13748-013-0040-3.

@article{
	year={2013},
	issn={2192-6352},
	journal={Progress in Artificial Intelligence},
	doi={10.1007/s13748-013-0040-3},
	title={Event labeling combining ensemble detectors and background knowledge},
	url={http://dx.doi.org/10.1007/s13748-013-0040-3},
	publisher={Springer Berlin Heidelberg},
	keywords={Event labeling; Event detection; Ensemble learning; Background knowledge},
	author={Fanaee-T, Hadi and Gama, Joao},
	pages={1-15}
}

## Contact

Created by [Pawan Mani Teja Kuppili](https://github.com/maniteja6799)