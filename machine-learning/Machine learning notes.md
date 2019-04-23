Section 1, lecture 4

Anaconda:
Python is the programming language and on top of that we can install different IDE's and packages and other modifications. Python by itself is not very useful to us without using an ID. Anaconda installs a few IDE's and most common packages like numpy, pandas and others. It's a convenient package basically. It contains Spyder which is out IDE of choice.

Section 2, Lecture 6

Data preprocessing:
This is to get data formatted correctly and prepared to ensure we build our machine learning models without issues.

Section 2, Lecture 7
https://www.superdatascience.com/machine-learning/

So the first dataset we have contains data of customers.
Independent variables are the first 3 variables, Country, Age and Salary and the dependent variables are the purchased (Yes/no).

So with machine learning we will use the first 3 columns of independent variables to predict whether or not the customer purchased the product.

Section 2, Lecture 8

3 essential libraries we will use every time. The first step of data preprocessing is to import the 3 essential libraries.
We do "import numpy as np" to import the library and use as to shorten how we reference it.

Numpy is a package which contains a lot of mathematical tools. Machine learning is based on mathmatics so we need numpy.

matplotlib.pyplot allows us to plot nice charts.

pandas is the best library to import and manage data sets. We will use it to import our data sets and later to manage them.

Section 2, lecture 9/10

Before starting to import a data set, we should specify a working directory folder which contains out data set. To specify, we save our python file to the working directory where the data set exists and we run the python file even if it's empty.

To run an import, we use pandas. We create a variable called dataset and run pd.read_csv('Data.csv'). Then we if look in our vairable explorer we should now see our imported CSV data. We can double click for more info.

We have our data set but we need to distinguish the matrix of features which is the
independent variables which will be all our observations so far. The dependent variable vector will be the purchased column.

To make our first matrix of features we run X = dataset.iloc[:, :-1].values
The first parameter (To the left of the comma) are the rows and on the second parameter (to the right of the comma) are the colums.
So our first : in iloc is saying we take all the lines. To the right we say :-1 which says all the columns except the last one (: = all and -1 is except last one.). .values just gets the values. Normal Python syntax.

When we run that line (Ctrl + enter) we can then type x in the console to see the object array. This shows the 3 first columns without purchased.

To create the dependent variable vector, we do the exact same thing but for the this vector we select only what we need. We need to select this based on the index which starts at 0 in python. So Starting from Country as 0, the column in index 3 is what we need. So our command becomes Y = dataset.iloc[:, 3].values

Python objects explanations:
https://hastebin.com/dasopihese.pas

Section 2, Lecture 11

One issue to deal with is missing data in a data set. We need to handle this problem for our machine learning model to work correctly. Looking at the data set, there's 2 missing data. One in age and Salary. We can simply remove lines but that could be dangerous as maybe this contains crucial information. A better idea is to take the mean of the columns. We will be using this now in Python.

We will use sklearn which contains amazing libraries for machine learning models. We use .preprocessing to preprocess data sets which is what we're doing. We want the Imputer class to take care of the missing data. The line "from sklearn.preprocessing import Imputer" creates us a class and from this we will create an object from this class.

We can use Ctrl + I to go to the object viewer/help which is like "inspect" for info about this class and find the parameters we need. First parameter is missing values which we call NaN to select empty fields.
Second parameter is strategy which is optional. We are using the mean here which is the default. We'll put it in for readability anyway. The next parameter is the axis which specifies whether we input rows or columns. We want an entire column so ours will be 0.
The next step is to fit the imputer object to our data set X. To do this, we can do:
imputer = imputer.fit(X[:, 1:3]) (Check 8 mins into vid for explanation of this)
The we are saying fit X's Age and Salary columns to imputer and select all rows. The : selects all rows and 1:3 selects columns in index position 1 and 2. We use 3 because the upper bound is included.

Now we just need to replace the missing columns with the mean.
X[:, 1:3] = imputer.transform(X[:, 1:3])
This will ensure that X (Our dataset) is transformed using the imputer.transform method with the data from those two columns. As we specified imputer will be using mean as a strategy, this is what will be applied.

Section 2, Lecture 12
We have two categorical variables because they simply contain categories. Country and Purchased are both categorical variables. Since we are using math here, it would cause issues trying to treat the strings as strings in maths so we are encoding the categorical variables into numbers.

LabelEncoder is a class from the sklearn preprocessing library. As we did before with imputer, we need to create a first object of the label encoder class.
labelencoder_X = LabelEncoder()
labelencoder_X.fit_transform(X[:, 0])
By typing these two lines of code, we fitted the labelencoder_X object to the first column country of our matrix X and the fit_transform method will return the encoded output.
Running this returns an output Out[8]: array([0, 2, 1, 2, 1, 0, 2, 0, 1, 0], dtype=int64) which is an encoded list of our countries.

Now we prepend "X[:, 0] = " to the beginning of the line to ensure we replace those rows with the output from the fit_transform method of the LabelEncoder class.

Now we hit a potential issue as these numbers have values and the values are equal in value to us but to the computer are not equal. France is not greater than Germany but when put into numbers, France being 1 and Germany being 0 means France is considered greater than Germany. To prevent this we will use "Dummy variables".
As we have 3 categories here, we will have 3 columns. In each column for each category there will either be a 1 or zero. If we are in the france columns for example, it will be 1 if the country is france or 0 if it is not france.

http://i.imgur.com/df4qSCU.png

To do this, we will use another class as LabelEncoder only encodes the values. we will use OneHotEncoder class by adding this to the end of our import for sklearn.preprocessing.

As we write onehotencoder = OneHotEncoder() we can use Ctrl + I to check what's required.
The second parameter is asking for categorical features. It wants the index of the columns we will OneHotEncode the categories for. This now becomes
onehotencoder = OneHotEncoder(categorical_features = [0])
Now we will fit this object we created for OneHotEncoder using the fit_transform and specifying X. We don't need to specify the columns as this was defined in our object. We add .toarray() method on the end.

This will OneHotEncode variable X. we can see this in the variable explorer. we change the format without decimals by changing the 3 to 0 to ensure no decimals from %.3f to %.0f
Looking at X and the dataset will show us X now contains the first column corresponds to france, the second columns to Germany and third to Spain.

Now to take care of the other categorical data (purchased) we only need LabelEncoder and not OneHotEncoder as since that is the dependant variable the machine leanring model will know it's the category and there's no order between the two.

We simply create a new labelencoder object to encode it to numbers again like we did with the countries and fit this to y.

Our code at this point looks like:
https://pastebin.com/haCePmNb

Section 2, Lecture 13

We need to split the data into a training set and a test set.
We will build the machine learning model on the training set and we will run our tests on the test set.

We will import a library to help us do this. We run from "sklearn.cross_validation import train_test_split"

We will create X_train, X_test, y_train, y_test.
The X_train is the training part of the matrix of features
X_test is the test part of the matrix of features
y_train is the training part of the dependent variables which is associated with y_train (same indexes and same observations as each other)
y_test is the test part of the dependent variable vector associated with X_test.

Those are the 4 variables we are creating at once. We will make them equal the value of train_test_split([method parameters]).

The method parameters can be seen as always using Ctrl + I. What it takes first is the arrays which will be X (the matrix of features for the independent variables) and y (the dependent variable). By putting X and y you are putting the whole data set in.
The next parameter is the test size. This is the size of the test set we want to use. If this is for example 0.5 then half of your data goes to the test set and half your data goes to the training set.
Normally we will use a smaller percentage than half. A good choice is generally 0.2 or 0.25 or rare cases will be higher.
Since we have 10 observations in our data set, once we split this, we will have 2 observations in the test set and 8 observations in the training set.

The next parameter is for the train size. This is irrelevant since we want the rest of the data in our training set we don't need to specify.
It also has a random_state parameter which is not always needed but it acts as a seed for generating random data so you can ensure the randomness is always equal (this means two people can run the same script and have the same "randomly" picked samples).

The training set is on what the machine learning model learns and the test set is on which we test if the machine learning model learned the correlations correctly.

Section 2, Lecture 14
Feature scaling.
We have two columns age and salary which contain numerican numbers. You notice they're not on the same scale. The age is going from 27 to 50 and the salary is going from 48k - 83k. As they're not on the same scale, this will cause some issues in our machine learning models.
The models are based on what is called the Euclidean Distance.
Here it's the same.
Since the salary has a much wider range of values, the Euclidian distance will be dominated by salary. The distance between the highest and lowest salary is 31,000. Age only has a difference of 21.
If we kept these in the same scale, the impact from age would be so insignificant it would hardly need to exist. This is why we need to scale them.

4:32 explains feature scaling methods.
http://i.imgur.com/a5dHLDD.png

Standardisation vs Normalisation.

The point is we put our variables in the same range in the same scale.

To scale features we will use the standard_scaler class from the sklearn.preprocessing library.

When you are applying your standardScaler object you have to fit the object to the training set.
We don't need to fit the test set as this is already fitted to the training set.

Two questions we should ask ourselves. Do we need to fit and transform our dummy variables? This takes either 0 or 1. Some people will say yes and some people say no.
