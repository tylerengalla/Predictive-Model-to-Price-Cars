# Predictive-Model-to-Price-Cars
Used R to compare Linear Regression and Random Forest models to predict car prices  

To predict the price of a car based on a set of the car’s features, the supervised learning method I chose was a Linear Regression model. This method was chosen as a straight forward means of getting to a predicted price based on 15 features: trim, subTrim, condition, isOneOwner, mileage, year, color, displacement, fuel, state, region, soundSystem, wheelType, wheelSize, featureCount.
The first step was to explore the data set. There was some pre-processing that needed to be done such as checking for missing values and converting the categorical variables into factors.
Once the data was prepped, the next step was to split the data into training and testing data sets so that we can see how the model performs against unseen data.
Then we run the linear model. This allows us to identify significant predictors and also refine the model as we go by excluding features based on contextual hunches and statistical significance.

## [1] "Linear RMSE: 10559.2843411631"
The first model was run with the full feature set and gave an Adjusted R-squared: 0.9428 and a RMSE of 10559. Meaning our model was able to account for ~95% of the variability involved in estimating the price, while on average the price that was predicted differed from the actual by roughly ~$10k.
Vairables with p-values less than .05 such as certain trim packages (trim420, trim63 AMG), whether the car is new or not, the mileage, year, and many specific displacement levels significantly impact price.
For example, having a trim420 package is associated with an increase of ~52,570 in price (holding other variables constant) and mileage, as expected, decreases prices by ~.13 with each mile.
Non-significant predictors that do not impact price included sound system, most states the vehicle is from, and color. 

But being off by $10k on average in the car market isn’t quite the accuracy we’d want when trying to accurately predict a car’s price.
Trying to improve on this model, I took more of an intuitive approach to see if we can vastly simplify our model and improve our RMSE by only taking variables that were statistically significant and what I perceived as generally the most important factors in buying a car - condition, mileage, and year of the vehicle.
This attempt at refining and simplifying our model reduced our Adjusted R-squared to 0.8726 and provided a RMSE of 15764. So we accounted for less of the variability and are now on average off by ~$16k. With an attempt to greatly simplify we have made the model worse at predicting the price.
The next iterative approach was to try and regularize the data using Lasso Regression. This method allows us to eliminate some of the features that are irrelevant. This produced a RMSE of 10565. However, this is still slightly higher than our original linear model. Looking at the plot of the residuals of the Lasso Regression, one of the reasons for this may be due to some non-linearity in the data. Additionally, we can notice the spread of residuals increase as the actual price increases (showing heteroscedasticity), meaning our model is not great at predicting expensive car prices.

## [1] "Lasso RMSE: 10565.9658579047"


In an alternative attempt to get a better predicting model of price, the next approach taken was with a non-linear model using a Random Forest.This will combine multiple decision trees in an effort to improve accuracy and account for outliers we’re seeing in the linear model.
The random forests provides us with a feature importance score that lets us know that trim is the most important feature, followed by state (however from our linear model we see that only state being unspecified or SD plays any statistical significance), year, displacement, and finally mileage in determining a car’s price. Intuitively, these features impacting price would all generally make sense.
The RMSE for the random forest was 6855 and when calculating the multiple-r squared value to compare it to our previous models, we get .9765. Meaning, with this model we can account for ~98% of the variability in price based on these 15 features and on average get that price right within $6900 (much better than our $10k earlier from the linear model). Looking at the residual plot, we notice a more uniform spread, but the model still struggles with accurately predicting the higher priced vehicles. But overall, the random forest provided the best performance at predicting a car’s price.

## [1] "Random Forest RMSE: 6855.24430003652"
