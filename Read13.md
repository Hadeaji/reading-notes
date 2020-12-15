# Linear Regressions

## How to run Linear regression in Python scikit-Learn

linear regression is a popular technique and you might as well seen the mathematical equation of linear regression.

There are several ways in which you can do that, you can do linear regression using numpy, scipy, stats model and sckit learn.

Scikit-learn is a powerful Python module for machine learning. It contains function for regression, classification, clustering, model selection and dimensionality reduction.

## Exploring Boston Housing Data Set

The first step is to import the required Python libraries into Ipython Notebook

```
import numpy as np
import pandas as pd
import scipy.stats as stats
import matpoltlib.pyplot as plt
import sklearn
```

import Boston data set into Ipython notebook and store it in a variable called boston.

```
from sklearn.database import load_boston
boston = load_boston()
```

The object boston is a dictionary, so you can explore the keys of this dictionary

predict the housing prices in boston region using the features given

`print(boston.DESCR)`

convert boston.data into a pandas data frame.

`bos =pd.Dataframe(boston.data)`

the column names are just numbers, so I am going to replace those numbers with the feature names.

```
bos.columns = boston.feature_names
bos.head()
```

## Scikit Learn

Y = boston housing price(also called “target” data in Python)

and

X = all the other features (or independent variables)

First, import linear regression from sci-kit learn module.

Then drop the price column as I want only the parameters as my X values.

store linear regression object in a variable called lm.

**Important functions to keep in mind while fitting a linear regression model are:**

- lm.fit() -> fits a linear model

- lm.predict() -> Predict Y using the linear model with estimated coefficients

- lm.score() -> Returns the coefficient of determination (R^2).

## Fitting a Linear Model

```
lm.fit(X, bos.PRICE)

LinearRegression(copy_X=True, fit_intercept=True, normalize=False)
```


then construct a data frame that contains features and estimated coefficients.

`pd.Dataframe(zip(X.columns, lm.coef_), columns =['features','estimatedcoefficients'])`

You will see from the data frame that there is a high correlation between RM and prices.

## Training and validation data sets

In practice you wont implement linear regression on the entire data set,you will have to split the data sets into training and test data sets.

So that you train your model on training data and see how well it performed on test data.

You can create training and test data sets manually, but this is not the right way to do,

because you may be training your model on less expensive houses and testing on expensive houses.

## How to do train-test split

You have to divide your data sets randomly. Scikit learn provides a function called `sklearn.model_selection` to do this.

## Conclusion

1. explore the boston data set and then renamed its column names.
2. use Scikit learn to fit linear regression to the entire data set and calculate the mean squared error.
3. make a train-test split and calculate the mean squared error for training data and test data.
4. then plot the residuals for training and test datasets.
