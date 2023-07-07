# Hurricane Impact on Florida Real Estate Value 

**Final Project: Analyzed the impact of five different hurricanes on Florida Real Estate value using data sourced from Zillow and The National Oceanic and Atmospheric Administration (NOAA).**

## Summary 

For my capstone project at Flatiron School I used machine learning and classification models to analyze hurricane impact on real estate value. Data from NOAA and Zillow were used to provide the features for the model. The target value was whether or not homes in a region increased in value (encoded as a boolean). If the percentage change in the value of a home was in the 75th percentile or greater six months after a hurricane it was considered to be in the category of increase. The model features were average wind speed (AWND), fastest 2 minute wind gust (WSF2), size rank of the city (SizeRank), and home value six months before the hurricane (before). 

## Objectives 

My main objective for this project was to help real estate companies understand how hurricanes impact real estate value in an area. Using machine learning classification algorithms I wanted to know if hurricanes hitting an area could be a predictor of home value increase. As a Floridian and survivor of hurricane Charley (2004) and hurricane Ian (2022) I have directly seen the impact hurricanes have on real estate and wanted to see if that could be modeled. The first and most direct impact is the destruction of homes and properties. Followed by the migration of people out of Florida who proceed to sell their homes, either as a result of increasing insurance prices or the trauma of surviving a hurricane. And while having to rebuild our communities is an arduous task–the silver lining is that properties and infrastructure are improved and updated, increasing both home and property value. 

## Data 

### Data Gathering

In total I used 9 datasets sourced from Zillow and NOAA. Data from Zillow was used in order to provide home value, city, size rank, and home value six months before and six months after the hurricanes. Data from NOAA was used to know the average wind speed and fastest 2 minute wind gust cities in Florida experienced during six hurricanes. Data from both sources was joined into a final dataset and used for the classification models. 

#### Zillow 

Data from Zillow's Home Value Index (ZHVI) was used. Zillow Home Value Index is a measure of a home's typical value and market changes across a given region and type of housing. By measuring monthly changes in property across different housing types and geographies Zillow is able to capture how the market price changes and not just the changes in the kinds of markets or property types that sell on a month to month basis. The ZHVI dollar amount is representative of the "typical home value for a region" and not the "median home value".

I looked at three different datasets from Zillow that had been smoothed and seasonally adjusted:

**Bottom Tier Homes**: typical value for homes within the 5th to 35th percentile range for a given region.        
**Middle Tier Homes**: typical value for homes within the 35th to 65th percentile range for a given region.     
**Top Tier Homes**: typical value for homes within the 65th to 95th percentile range for a given region.     

#### NOAA 

Data was obtained from the National Oceanic and Atmospheric Administration (NOAA) and National Climatic Data Center (NCDC) using the Climate Data Online (CDO) database. The CDO provides free access to NCDC's archive of global historical weather and climate data in addition to station history information. These data include quality controlled daily, monthly, seasonal, and yearly measurements of temperature, precipitation, wind, and degree days as well as radar data and 30-year Climate Normals. For the purpose of this project all data was downloaded in CSV format, but PDF and Daily Text was available as well. 

Data from NOAA was selected because it provides daily summaries for average wind speed and fastest 2 minute wind gusts for the six hurricanes I wanted to examine.

For the purpose of this project I looked at six hurricanes: Charley (08/2004), Dennis (07/2005), Matthew (10/2016), Irma (09/2017), Michael (10/2018), and Ian (09/2022)

I used information from hurricane Charley, Dennis, Matthew, Irma, and Michael to create classification models and validated the model with recent data from hurricane Ian.

### Final Dataset 

I used three datasets for the purpose of modeling. The datasets each contained the same 7 columns: 
AWND: Average daily wind speed (miles per hour)    
WSF2: Fastest 2-minute wind speed (miles per hour)   
SizeRank: Numerical rank of size of cities, ranked 0 through 30,132   
before: Home value six months before the hurricane    
after: Home value six months after the hurricane     
percent: Percent change in home value from six months before hurricane to six months after hurricane    
increase: 1 = increase of 75% of more in home value, 0 = no increase of 75% of more in home value    
I trained the models on data drawn from the bottom tier home value dataset which had 141 entries. I chose this dataset to train our models on because it had the best target variable class imbalance of  68%. I used our dataset containing all home values to test our model which contained 344 entries. I also used a dataset containing just data from hurricane Ian to see how the model would perform on future hurricane data. The hurricane Ian dataset contained 27 entries. 

## Feature Engineering 

In order to create our models I needed to engineer two additional features: home value increase and city. 

### Using GeoPy to Get Cities

Data from NOAA provided location information in the form of latitude and longitude while data from Zillow provided location information in the form of city. In order to join the data from NOAA to the data from Zillow I needed to know the city names. Using the coordinates provided by the NOAA dataset I used GeoPy to reverse geolocate the city names.

### Home Value Increase

Since this was a classification problem the target variable had to be in the form of a class. Using home value data from six months before and after each hurricane I engineered a column that contained two categories, increased significantly (coded as 1) and did not increase significantly (coded as 0). If the percentage change in the value of a home was in the 75th percentile six months after a hurricane it was considered to be in the category 1, if not then 0. 

## Modeling 

![Hurricane Impact on Florida Real Estate Confusion Matrix](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/Confusion_Matrix.jpg)

Of equal concern to the business problem was the model predicting False Negatives or False Positives. In this situation both are of equal importance to our real estate client. If the model predicts that the home value will not increase but it does (false negative) then our client could lose out on a possible higher return on a home. However, if the model predicts that the home value will increase but it does not (false positive) then our client may have invested money into a home that does not pay off. 

### Metrics 

The metrics used were accuracy and F1 Score to assess model performance. Since there was a class imbalance I could not solely rely on accuracy to communicate model performance. Since, precision and recall were of equal importance for our business problem F1 score was used to assess model performance. The model’s AUC-ROC curve was also assessed to see how well the model performed. An excellent model has an AUC near 1, while an AUC near 0 means the model is actually reciprocating the classes. 

### Logistic Regression 

I chose to use a logistic regression model because it is commonly used for prediction and classification problems. The model was iterated through with our scaled data. Iterations were also performed after removing collinear variables and using SMOTE to adjust for the class imbalance. The logistic regression model that performed the best was our SMOTE model with no collinear features and had an accuracy of 0.74 which was slightly better than our baseline accuracy of 0.68 and an F1 score of 0.68. It found that AWND was the most important feature in predicting home value increase. 

### XG Boost 

An XG Boost model was used due to its adaptability and strong prediction performance. Our XG Boost model had perfect accuracy and a perfect F1 score. When the model was run without the inclusion of wind features model accuracy dropped to 98% and the F1 score dropped to 0.96. Model tuning was also performed on a model that contained just wind features, however, it did not improve overall model performance. 

### Final Model Validation 

Our final model was an XG Boost model. Since the goal for this project is to help real estate agencies understand how hurricanes impact home value data from a recent hurricane was used to test our model. Using data from hurricane Ian our model performed with 74% accuracy, which is slightly better than the baseline accuracy of 59%. The F1-score was 0.544 and the model predicted 7 false negatives and no false positives. The AUC score was 0.92. 

## Recommendations 
I recommend further looking into cities that have large Size Ranks and lower than average housing prices as a predictor, regardless of if a hurricane happened because it is a strong indicator of home value. Hurricane features do slightly improve model performance and should also be considered. 

Wind speed features have a correlation that is similar to that of SizeRank and before prices, hence, it may be an indicator of home value increase and it is relatively relevant to models predictions. 

I recommend using the model to attempt to buy homes after the next hurricane to see if the model is effective in making business decisions. 


## Further Improvements 
Further improvements for this project would be to continue collecting data. For the scope of this project I looked at all hurricanes that were a category 4 or above for the last twenty years. I recommend continuing at all hurricane data that also has real estate data available. 

I also recommend looking at a different region which is impacted by hurricanes, such as Texas, to see what other features could be of importance while modeling. 

## References 

## Reproduction Instruction

## Contact Me 
Thank you for checking out my GitHub if you have any further questions please contact me at samgrasland@gmail.com! 

## Repo Structure 



