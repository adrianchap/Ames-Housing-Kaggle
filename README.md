# Project 2 - Ames Housing Data and Kaggle Challenge

## Ames Housing Data Regression
by Adrian Chapman

### Problem Statement

The Ames housing data contains information on houses sold in Ames, IA from the years 2006 to 2010.  Many features reagarding the quality, size, age, and location of the homes are included - totaling 82 variables for modeling. This project applies advance regression techniques to determine what regression methodologies are successful in predicting housing sale price from this data.  In addition, interpretable and actionable insights for current and potential homeowners are discussed.  

--- 

### Data Dictionary

The data dictionary for the Ames housing data can be found [here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt).

In total, there were 23 nomina, 23 ordinal, 14 discrete, and 20 continuous variables to model with.  

---

### Data Cleaning

All data cleaning steps are carried out in the file **01_EDA_and_Cleaning** in the *code* directory of this repository.  This file takes in the 'train.csv' and 'test.csv' files to produce clean datasets for testing: 'train_clean_features.csv; and 'test_clean_features.csv', in the *datasets* directory. Cleaning steps for each variable type are outlined below:

Continouous Variables: 
- Assumed missing values meant the feature didn’t exist for that property for ‘Lot Frontage’ and ‘Mas Vnr Area’ and set missing values to 0.
Discrete Variables:
- Filled missing values for ‘Bsmt Full Bath’, ‘Bsmt Half Bath’ to zero.
- Imputed ‘Garage Yr Blt’ based on the average value for houses built in the same year. 
- ‘Mo Sold’ and ‘Yr Sold’ were treated as Nominal variables.
Nominal Variables:
- Any missing values were given the label ‘None’. 
- ‘MS SubClass’ was converted from an integer to a string.
- All nominal variables were one hot encoded into dummy columns
Ordinal Variables:
- All ordinal variables were assigned positive numerical values based on hierarchical information provided in the data dictionary.
- For example, the ‘Bsmt Qual’ variable was recoded so that ‘Poor’ was assigned a value of 1 and ‘Excellent’ was assigned a value of 5.

Additional steps:
- Outliers: Removed two rows with ‘Gr Liv Area’ greater than 4,000 square feet, following guidance from data dictionary.
- Missing Data: Two rows were also removed for having significant missing data for several columns. 

---

### Feature Engineering and Preprocessing

Feature engineering and preprocessing steps were carried out in the **02_Preprocessing_and_Feature_Engineering** file in the *code* directory.  This file takes in 'train_clean_features.csv' from the *datasets* directory and after transforming additional features produces the 'X_y_features.csv' file in the *datasets* directory.

Three new features were created to ease interpretability of analysis:
- ‘Total Bath’: sum of all bathrooms in the house with a .5 weight on half baths
- ‘Time Since Remodel’: The difference between ‘Year Sold’ and ‘Year Remodel/Add’
- ‘Age when sold’: The difference between ‘Year Sold’ and ‘Year Built 
Three interaction features were added to the dataset as well:
- 'Overall Qual * Gr Live Area'
- 'Overall Qual * Year Built'
- 'Overall Qual * Remodle/Add'

In addition, our target variable 'SalePrice' was log transformed to produce a more normal distribution.

---

### Analysis

Several types of regression are performed in the **03_Modeling_and_Submissions** file in the *code* directory.  This file uses the 'X_y_features.csv' file found in the *datasets* directory.  After producing a suitable regression model, the file takes in 'test_clean_features.csv' file in the *datasets* directory to perform the same transformations on the test dataset that were peformed on the training dataset.  This is the test data used for a [kaggle competition] (https://www.kaggle.com/c/dsi-us-13-project-2-regression-challenge/overview).  The file also produces .csv files in the correct format for kaggle submission.  All submissions made to kaggle are found in the *submissions* folder.

Three types of regression were used in this study

**Ridge Regression:** The model with the lowest Root Mean Squared Error (RMSE) for the Kaggle submission was a Ridge Regression which iterated over 100 values to optimize alpha.
- R2 for Training set: 0.93
- R2 for Test set: 0.92
- The RMSE for the Test Set: $21,075
This model explains 93% of the variance of the Sales Price for our test data.  The high and similar R2 for Train and Test indicate this model should perform well on unseen data.

**Lasso Regression:** Despite not performing as well on the Kaggle set, Lasso was used to help determine features most likely to be useful for a more interpretable linear model. 
- R2 for Training set: 0.93
- R2 for Test set: 0.93
- The RMSE for the Test Set: $20,909
This plot shows the highest Lasso coefficient values after regularization.  These features were used to develop the final (and most interpretable) linear model.

**Linear Regression:** Linar Regression was used on the set of features with the highest coefficients from the Lasso Regularization performed.  This modle was ued to produce the most interpretable and actionable coefficients.
- R2 for Training set: 0.91
- R2 for Test set: 0.91
- The RMSE for the Test Set: $23,423
This model still explains 91% of the variance for Sale Price on the testing data, and does not show signs of overfitting.  Furthermore, the coefficients provide meaningful information about how much additional value each feature provides when controlling for all other variables.

---

### Conclusions and Recommendations

With appropriate data cleaning techniques, regularized regression performs well fitting the Ames housing data set to Sale Price with minimum feature engineering.

Linear Regression without regularization provides quantifiable insights for homeowners and potential buyers:
- Neighborhoods matter - with some neighborhoods commanding a $100,000 price premium, all else held constant
- Recent Remodels, Add Ons, Basement finishes can improve the value of the home
- Space is at a premium - over $50 per square feet
- Projects improving the heating or exterior quality of the home are probably worth it, but specific home improvement costs can be checked against model coefficients to evaluate return on investment 


