# Hurricane Impact on Florida Real Estate Value 

**Final Project: Analyzed the impact of five different hurricanes on Florida Real Estate value using data sourced from Zillow and The National Oceanic and Atmospheric Administration (NOAA).**
![Home Impacted by Hurricane Ian](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/banner.png)

## Summary 

For my capstone project at Flatiron School I used machine learning and classification models to analyze hurricane impact on real estate value. Data from NOAA and Zillow were used to provide the features for the model. The target value was whether or not homes in a region increased in value (encoded as a boolean). If the percentage change in the value of a home was in the 75th percentile or greater six months after a hurricane it was considered to be in the category of increase.  The model features were average wind speed (AWND), fastest 2 minute wind gust (WSF2), size rank of the city (SizeRank), and home value six months before the hurricane (before). The features AWND and WSF2 were used to assess how severely a city had been impacted by a hurricane. 

## Objectives 

My main objective for this project was to help real estate companies understand how hurricanes impact real estate value in an area. Using machine learning classification algorithms I wanted to know if hurricanes hitting an area could be a predictor of home value increase. As a Floridian and survivor of hurricane Charley (2004) and hurricane Ian (2022) I have directly seen the impact hurricanes have on real estate and wanted to see if that could be modeled. The first and most direct impact is the destruction of homes and properties. Followed by the migration of people out of Florida who proceed to sell their homes, either as a result of increasing insurance prices or the trauma of surviving a hurricane. And while having to rebuild our communities is an arduous task–the silver lining is that properties and infrastructure are improved and updated, increasing both home and property value. 

## Data 

### Data Gathering

In total I used 9 datasets sourced from [Zillow](https://www.zillow.com/research/data/) and [NOAA](https://www.ncei.noaa.gov/cdo-web/search?datasetid=GHCND). Data from Zillow was used in order to provide home value, city, size rank, and home value six months before and six months after the hurricanes. Data from NOAA was used to know the average wind speed and fastest 2 minute wind gust cities in Florida experienced during six hurricanes. Data from both sources was joined into a final dataset and used for the classification models. 

#### Zillow 

Data from [Zillow's Home Value Index (ZHVI)](https://www.zillow.com/research/data/) was used. Zillow Home Value Index is a measure of a home's typical value and market changes across a given region and type of housing. By measuring monthly changes in property across different housing types and geographies Zillow is able to capture how the market price changes and not just the changes in the kinds of markets or property types that sell on a month to month basis. The ZHVI dollar amount is representative of the "typical home value for a region" and not the "median home value".

I looked at three different datasets from Zillow:

**Bottom Tier Homes**: typical value for homes within the 5th to 35th percentile range for a given region.        
**Middle Tier Homes**: typical value for homes within the 35th to 65th percentile range for a given region.     
**Top Tier Homes**: typical value for homes within the 65th to 95th percentile range for a given region.     

![Home Value Change](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/Bottom_house_graph.png)

#### NOAA 

Data was obtained from the National Oceanic and Atmospheric Administration (NOAA) and National Climatic Data Center (NCDC) using the [Climate Data Online (CDO) database](https://www.ncei.noaa.gov/cdo-web/search?datasetid=GHCND). The CDO provides free access to NCDC's archive of global historical weather and climate data in addition to station history information. These data include quality controlled daily, monthly, seasonal, and yearly measurements of temperature, precipitation, wind, and degree days as well as radar data and 30-year Climate Normals. For the purpose of this project all data was downloaded in CSV format, but PDF and Daily Text was available as well. 

Data from NOAA was selected because it provides daily summaries for average wind speed and fastest 2 minute wind gusts for the six hurricanes I wanted to examine.

For the purpose of this project I looked at six hurricanes: Charley (08/2004), Dennis (07/2005), Matthew (10/2016), Irma (09/2017), Michael (10/2018), and Ian (09/2022). I used information from hurricane Charley, Dennis, Matthew, Irma, and Michael to create classification models and validated the model with recent data from hurricane Ian.
![Box And Whisker](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/box_and_whisker.png)
![Hurricane Wind Map](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/C2DA9FD2-3EAE-434F-AAA6-62460FBD4005.jpg)
![Hurricane Wind Map](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/F7203A12-45AB-4811-81D6-A8DC97B53E34.jpg)

### Final Dataset 

**City**: City name   
**HurricaneName**: Name of the hurricane     
**AWND**: Average daily wind speed (miles per hour)      
**WSF2**: Fastest 2-minute wind speed (miles per hour)   
**SizeRank**: Numerical rank of size of cities, ranked 0 through 30,132   
**before**: Home value six months before the hurricane    
**after**: Home value six months after the hurricane     
**percent**: Percent change in home value from six months before hurricane to six months after hurricane    
**increase**: 1 = increase of 75% of more in home value, 0 = no increase of 75% of more in home value  

I trained the models on data drawn from the bottom tier home value dataset which had 141 entries. I chose this dataset to train our models on because it had the best target variable class imbalance of  68%. I used the dataset containing all home values to assess the impact of wind features and tune the model which contained 344 entries. I also used a dataset containing just data from hurricane Ian to see how the model would perform on future hurricane data. The hurricane Ian dataset contained 27 entries. 

![Variable Correlation](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/BottomCorrHeatMap.png)
![Pairplot](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/BottomPairplot.png)

## Feature Engineering 

In order to create our models I needed to engineer two additional features: home value increase and city. 

### Using GeoPy to Get Cities

Data from NOAA provided location information in the form of latitude and longitude while data from Zillow provided location information in the form of city. In order to join the data from NOAA to the data from Zillow I needed to know the city names. Using the coordinates provided by the NOAA dataset I used GeoPy to reverse geolocate the city names.

### Home Value Increase

Since this was a classification problem the target variable had to be in the form of a class. Using home value data from six months before and after each hurricane I engineered a column that contained two categories, increased significantly (coded as 1) and did not increase significantly (coded as 0). If the percentage change in the value of a home was in the 75th percentile six months after a hurricane it was considered to be in the category 1, if not then 0. 

![Class Imbalance](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/bottomtierclassimbalance.png)

## Modeling 

![Hurricane Impact on Florida Real Estate Confusion Matrix](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/Confusion_Matrix.jpg)

Of equal concern to the business problem was the model predicting False Negatives or False Positives. In this situation both are of equal importance to our real estate client. If the model predicts that the home value will not increase but it does (false negative) then our client could lose out on a possible higher return on a home. However, if the model predicts that the home value will increase but it does not (false positive) then our client may have invested money into a home that does not pay off. 

### Metrics 

The metrics used were accuracy and F1 Score to assess model performance. Since there was a class imbalance I could not solely rely on accuracy to communicate model performance. Since, precision and recall were of equal importance for our business problem F1 score was used to assess model performance. The model’s AUC-ROC curve was also assessed to see how well the model performed. 

#### Accuracy 
Model accuracy is a machine learning classification model performance metric that takes the ratio of true positives and true negatives to all positives and negative results. It communicates how often our model will correctly predict an outcome out of the total number of predictions made. However, accuracy metrics are not always reliable for imbalance datasets. Accuracy Score = (TP + TN)/ (TP + FN + TN + FP)


#### F1 Score 
Model F1 Score is a model performance metric that gives equal weight to both the Precision and Recall for measuring performance. F1 Score = 2* Precision Score * Recall Score/ (Precision Score + Recall Score/)

#### ROC 
The Receiver Operator Characteristic (ROC) curve is used to assess a model’s ability to correctly classify by plotting the true positive rate against the false positive rate. A curve that ‘peaks’ more quickly communicates that there is a good true positive rate and a low false positive rate. The area under the curve (AUC) is derived from ROC and has a baseline chance of 50% accuracy, hence, an AUC closer to 1 signifies a better classification model. 

### Baseline
As a baseline I looked at the class imbalance of the target variable for the various datasets. This allowed me to know if the model accuracy was any better than just selecting the majority class.  

![Baseline](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/classimbalancechart.jpg)

### Logistic Regression 

I chose to use a logistic regression model because it is commonly used for prediction and classification problems. The model by default was set with an L2 penalty. It was iterated through the scaled data. Iterations were also performed after removing collinear variables and using SMOTE to adjust for the class imbalance. The logistic regression model that performed the best was the SMOTE model with no collinear features and had an accuracy of 0.74 which was slightly better than our baseline accuracy of 0.68 and an F1 score of 0.68. It found that AWND was the most important feature in predicting home value increase. 

### XG Boost 

An XG Boost model was used due to its adaptability and strong prediction performance. Our XG Boost model had perfect accuracy and a perfect F1 score. When the model was run without the inclusion of wind features model accuracy dropped to 98% and the F1 score dropped to 0.96. Model tuning was also performed on a model that contained just wind features, however, it did not improve overall model performance. 

![Confusion Matrix](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/confusion_matrix%20for%20best%20model.png)
![Feature Importance](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/SHAPIan.png)

### Final Model Validation 

Our final model was an XG Boost model. Since the goal for this project is to help real estate agencies understand how hurricanes impact home value data from a recent hurricane was used to test our model. Using data from hurricane Ian our model performed with 74% accuracy, which is slightly better than the baseline accuracy of 59%. The F1-score was 0.544 and the model predicted 7 false negatives and no false positives. The AUC score was 0.92. 

![Hurricane Ian Confusion Matrix](https://github.com/SSGrasland/Modeling-Hurricane-Impact-on-Florida-Real-Estate-Value-/blob/main/data/images/confusion%20matrix%20for%20ian.jpg)

## Recommendations 
I recommend further looking into cities that have large Size Ranks and lower than average housing prices as a predictor, regardless of if a hurricane happened because it is a strong indicator of home value. Hurricane features do slightly improve model performance and should also be considered. 

Wind speed features have a correlation that is similar to that of SizeRank and before prices, hence, it may be an indicator of home value increase and it is relatively relevant to models predictions. 

I recommend using the model to attempt to buy homes after the next hurricane to see if the model is effective in making business decisions. 

## Further Improvements 
Further improvements for this project would be to continue collecting data. For the scope of this project I looked at all hurricanes that were a category 4 or above for the last twenty years. I recommend continuing at all hurricane data that also has real estate data available. 

I also recommend looking at a different region which is impacted by hurricanes, such as Texas, to see what other features could be of importance while modeling. 

## References 

Centers N. Select a Location | Data Tools | Climate Data Online (CDO) | National Climatic Data Center (NCDC). Noaa.gov. Published 2019. https://www.ncdc.noaa.gov/cdo-web/datatools/selectlocation     
Classification Feature Selection : SHAP Tutorial. kaggle.com. Accessed July 6, 2023. https://www.kaggle.com/code/ritzig/classification-feature-selection-shap-tutorial#SHAP       
Filho M. How To Get Feature Importance In Logistic Regression. forecastegy.com. Published March 30, 2023. Accessed July 10, 2023. https://forecastegy.com/posts/feature-importance-in-logistic-regression/#feature-importance-in-binary-logistic-regression       
Flatiron School. Flatiron School. Accessed July 7, 2023. https://learning.flatironschool.com/courses/5333          
Hurricanes. coast.noaa.gov. https://coast.noaa.gov/hurricanes/  
Kumar A. Accuracy, Precision, Recall & F1-Score - Python Examples. Data Analytics. Published October 1, 2021. https://vitalflux.com/accuracy-precision-recall-f1-score-python-example/        
kumar_satyam. Get the city, state, and country names from latitude and longitude using Python. GeeksforGeeks. Published October 15, 2020. https://www.geeksforgeeks.org/get-the-city-state-and-country-names-from-latitude-and-longitude-using-python/              
National Centers for Environmental Information (NCEI. Climate Data Online (CDO) - The National Climatic Data Center’s (NCDC) Climate Data Online (CDO) provides free access to NCDC’s archive of historical weather and climate data in addition to station history information. | National Climatic Data Center (NCDC). Noaa.gov. Published 2019. https://www.ncdc.noaa.gov/cdo-web/         
Olsen S. Zillow Home Value Index Methodology, 2023 Revision: What’s Changed? Zillow. Published February 11, 2023. https://www.zillow.com/research/methodology-neural-zhvi-32128/       
Training, Tuning, and Test Datasets. onepager.togaware.com. Accessed July 6, 2023. https://onepager.togaware.com/Training_Tuning_Test_Datase.html            
ZHVI User Guide. Zillow Research. https://www.zillow.com/research/zhvi-user-guide/        
Zillow. Housing Data - Zillow Research. Zillow Research. Published 2011. https://www.zillow.com/research/data/         

## Repo Structure and Reproduction Instructions

### Structure 
|-- data <- Folder containing datasets used in analysis, images, and documentation     
|--  presentation and notebook pdf <- Folder containing presentation and notebook pdf              
|-- .gitignore <- File that ignores files     
|-- 1.HousingData.ipynb <- Narrative documentation of analysis in Jupyter notebook     
|-- 2.HurricaneData.ipynb <- Narrative documentation of analysis in Jupyter notebook             
|-- 3.JoiningDatasets.ipynb <- Narrative documentation of analysis in Jupyter notebook         
|-- 4.Modeling.ipynb <- Narrative documentation of analysis in Jupyter notebook         
|-- 5.WindSpeedAnalysisandModelTuning.ipynb <- Narrative documentation of analysis in Jupyter notebook            
|-- README.md <- The top-level README for reviewers of this project            
└ - environment.yml <- Environment used in this project       

### Reproduction 
All data used in this project is available in the data folder. The data can be processed using the notebooks in numerical order. The environment used is available as a .yml file. 

#### Downloading the Data
###### NOAA 
If you would like to download the data from the original source please visit the [Climate Data Online Search](https://www.ncei.noaa.gov/cdo-web/search?datasetid=GHCND). And set the boxes as follows: 
- Select Weather Observation Type/Dataset: Daily Summaries       
- Select Date Range: Use the date of the hurricane you would like to look at (i. e. Ian 9/28/2022 to 9/29/2022)       
- Search For: States       
- Enter a Search Term: Florida      
Then add the dataset to cart and checkout. Download as CSV and adjust dates. Unders Station Detail check Geographic Location and Select Datatypes of AWND and WSF2. Then submit the order and receive it to your email. 
###### Zillow 
If you would like to download the data from the original source please visit [Zillow’s Housing Data](https://www.zillow.com/research/data/). Under the Home Values section can be downloaded the three datasets used for this project, please assure to select city: 
- ZHVI All Homes- Top Tier Times Series ($)     
- ZHVI All Homes - Bottom Tier Times Series ($)      
- ZHVI All Homes (SFR, condo, Co-op) - Times Series, Smoothed, Seasonally Adjusted ($)        

## Libraries 
**Pandas**: Python Data Analysis Library     
**NumPy**: For working with arrays       
**Matplotlib**: For visualization      
**Seaborn**: For visualization             
**SHAP**: Used to explain output of machine learning models                  
**Scikit Learn**: For machine learning                
**Scipy**: For scientific and mathematical problems                 
**XGBoost**: Optimized distributed gradient boosting library              
**Geopy**: Used for locating addresses from coordinates                

## Contact Me 
Thank you for checking out my GitHub if you have any further questions please contact me at samgrasland@gmail.com! 







