# Housing Prices Regression Model
General Assembly Data Science Immersive Project - Multiple Linear Regression Housing Price Cohort Competition
# Overview

   This project includes the first model I built as part of the General Assembly Data Science Immersive program. The project was a competition to predict prices in Ames, IA housing data and is based on a prior [kaggle competition](https://www.kaggle.com/c/house-prices-advanced-regression-techniques). This competition included ~100 total immersive students whose goal was to achieve the lowest possible root-mean-squared-error (RMSE), and was conducted across weeks 2 and 3 of the 12-week program. This project demonstrates my ability and passion for conducting thorough EDA, feature engineering, and feature selection ahead of model building. As a result, I was able to ensure low variance on testing data used to score models, and placed 7th in lowest RMSE. 

Note: 

   At this point in the curriculum, we covered all of the basics of python programming and were introduced to the linear regression model in sk-learn. As such, more advanced modeling techniques (PCA, hyper-parameter tuning, spline interpolation, bagging/boosting, gradient descent, forward/backward feature selection, etc) and types (decision trees, support vector machines, etc) were not included, though would definitely improve model performance.

# Dataset Description
   The original Ames, IA housing data contains the sale of 3,970 houses between 2006 and 2010. Detailed information is captured across 80 features including a mix of continuous, discrete, categorical, and binary features. At a high level, these features include various square footage measurements (above vs below ground, lot size), room counts and quality (bedrooms, bathrooms, basement), utilities, construction detail (eg roof material, siding), luxuries (eg fireplaces, pool), and many others. The full list of features and a data dictionary for the available values within each feature can be seen in the Data Fields and Detailed Data Dictionary files respectively.

# Methodology
4 key steps were taken to build the multiple linear regression model. Highlights on methodology and results from each step here, with more detail throughout relevant notebooks
  1. Data Import and Cleaning 
      
   A variety of columns contained missing values. Missing values for categorical variables were replaced with "NA" or "missing" while continuous variables were typically replaced with 0. Additional detail can be seen within the notebook. More advanced data imputation was not considered as it was not learned at this point in the curriculum. Of the 2,930 houses in the training dataset, the overwhelming majority of values (~2000) for Alley were missing, which makes this field somewhat limited in usefulness. Lot frontage also had a significant number of missing values (490). Though basement features had a small number of missing features (all under 60), these features were important in predicting housing prices, and as such more advanced imputation may be valuable. 
  
  2. Exploratory Data Analysis
  
   Similar features were analyzed side by side (eg all utilities together, all room counts/quality together). All features were analyzed either via scatterplots (continuous) or swarmplots (discrete/categorical). Strength of features was gauged by level of correlation vs price (continuous) or level of separation from other categories (discrete/categorical). By analyzing similar features side by side, risk of multicollinearity was minimized (eg various bathroom features are highly correlated with each other)
  
  3. Feature Engineering
  
   Two key types of feature engineering were performed:
      
   a) Dummy Variable Creation:
      
   For all categorical variables, the distribution of sales price within each option in a feature was analyzed. In cases where different levels had similar distributions of sales price, these levels were combined together when creating dummy variables.  Levels were also combined together when a given option had very few data points and therefore may not hold predictive power in testing data. Dummy variables were not always created for each category within a feature, but instead were created to prioritize levels within a feeature that had significantly higher or lower sales prices than the mean.
      
   b) Interaction Terms
      
   Often, related features provided addtional predictive value when interacted with each other during the process of dummy creation. For example, garage capacity (number of cars), garage finish, and garage location relative to the house were all individually valuable towards predicting prices. These were then combined into one set of features that represents unique combinations of each of these 3 features. 
      
  4. Model Building
  
   A linear regression model was built by adding in one or few features at a time, in order of estimated predictive strength. Square footage, overall quality, bathroom, kitchen, and garage features were added first. With just these alone, R^2 of 0.84 and RMSE of 30.9K on training data was achieved. A wide variety of other created features (neighborhood, foreplace, foundation, land contour, MS subclass, functionality, sale type, etc.) were added in after, and brought R^2 up to 0.88 and RMSE on training data down to 25.3K


# Results and Analysis

   Residuals were larger and more positive for houses with higher real sales prices. In other words, the model struggled to accurately distinguish between moderately priced and highly priced homes. I believe this is true for a few reasons. First, there are very few high priced homes to begin with. Second, categorical features often had a majority of houses fall into one or a few values with a wide sales distribution, and a minority fall into values with a more condensed distribution at a lower price. These types of features are much better at identifying low-priced houses, while there are very limited number of features for whom a particular value contains a narrow distrbition of only high-priced homes. For instance, all homes priced above 400,000 have at least a 2 car garage, with the overwhelming majority haveing at least 3 car garages. However, the distribution of prices within houses of 3 car garages is somewhat evenly distributed across a range of 100,000 - 600,000. As such, houses with < 2 car garages can confidently be predicted to have lower prices, while the uncertainty of price for those with higher garage capacities is much broader. 
  
   To improve upon this analysis beyond general model techniques and types mentioned in the overview above, more robust exploration of interaction terms could be explored to identify more accurately separate moderately priced from high priced homes
