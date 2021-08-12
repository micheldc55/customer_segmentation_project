# Customer Segmentation Project

## Project Summary

In this project I create a customer segmentation model for Data obtained from Kaggle. Within the project I explore the features, visualize them, apply normalizations, review the data, create clusters, evaluate them and apply business-intelligence to the results.

The purpose of this project was to explore customer segmentation tools through clustering, using evaluation metrics and customer understanding through visualization to create insightful groups of customers that could be exploited in online marketing processes or similar tasks that require your customers to be grouped by their features.

# Table of Contents:

- [Project Objective](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#project-objective)
- [Process Overview](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#process-overview)
- [Detailed Process](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#detailed-process)
  - [Libraries used](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#libraries-used)

## Project Objective

The objective of this project was to use an unknown dataset and apply insights gained from the Data itself and the clustering models to obtain customer groupings that would provide interesting shared characteristics within each group that could be exploited. I believe the most captivating part about this project was the evaluation and interpretation process, as it challenges not only the methods used, but also the understanding of the data.

## Process Overview

During the process of obtaining the customer groupings (or clusters) I went through a step-by-step process which allowed me to go from importing the data to finally interpreting the clusters in a continuous process. This doesn't mean that this is the only way to conduct a customer segmentation process, but the steps taken here are a good way to organize your process if you haven't done any clustering or segmentation before. 

**The steps taken through this process were:**

1) Import the libraries you are going to use
2) Import the Dataset
3) Summary statistics for the dataset
4) Dealing with missing values
5) Condusting EDA & Visualizations
6) Understanding EDA (conclusions about the data)
7) Feature Transformations & Scaling
8) Modelling
    + Selecting Number of clusters
    + Evaluating Model
9) Dimensionality reduction for visualization
10) Interpretation of results

## Detailed Process 

### Libraries used

For this project, I used the libraries detailed below. On the left side of the table you can see the Python library used, and on the right column under 'Purpose' you can see for what it was used for.

| Library/Tool | Purpose |
| ------------- | ------------- |
| Matplotlib | Data Visualizations |
| Seaborn | Data Visualizations |
| Pandas | Data Management |
| Numpy | Data Management/Transformations |
| Scipy | Normality Tests and T-Tests of the Data |
| Scikit-learn's PowerTransformer | Data Transformations |
| Scikit-learn's MinMaxScaler | Feature Scaling |
| Scikit-learn's KMeans | Feature Clustering |
| Scikit-learn's Sillhouette metrics | Model Evaluation |
| Scikit-learn's PCA Decomposition | Cluster Visualization |
| Plotly offline | Interactive Cluster Visualization |
| Scikit-learn's DecissionTreeClassifier | Cluster Interpretation |

After conducting the necessary imports, we begin working with the data by importing the dataset using pandas. The file is a .csv containing information about the customers' age, income, gender, marital status, education level, occupation, and size of city they live in.

### Summary statistics about the data

I always like to get a general sense of the data as soon as I create the DataFrame from the data. Using the two methods that provide general statistics about the data (.info() and .describe()) we can get a general idea of the summary statistics of the data (mean, median, min, max and counts). the type of variables we are dealing with and null values.

Using the .describe() method for the data we realize that the age of the customers is restricted to the range between 18 and 76. We also notice that the income variable seems to have some outlier values based on the common reference value of: **Mean** + 1.5 * **IQR**. We also notice there are no missing values, or at least there aren't any null values within the dataset.

### EDA: Exploratory Data Analysis (general conclusions)

When we begin our EDA the first thing we need to do is visualize the data on its own to get a sense of the distribution if it is a numerical feature, and to get a sense of the predominance of each of the categories if it is a categorical feature.

#### Numerical Features:

When we plot both numerical features it becomes obvious that both distributions show a strong skew to the right, which is common amongst income and age data, due to it having a cut off point at zero.

**Age feature distribution:**

![plot1](https://github.com/micheldc55/customer_segmentation_project/blob/main/Age.png)

**Income feature distribution:**

![plot2](https://github.com/micheldc55/customer_segmentation_project/blob/main/Income.png)

These features need to be preprocessed in some way. We solved this by using different types of transformations (mainly Logarithmic transformations and scikit learn's PowerTransformer) in order to remove the strong right skew.

#### Categorical features:

We conduct the same process for categorical features, and obtain the countplots for each. An example of what these countplots looks like is shown below. Running univariant countplots, we reach the following conclusions:

- Customer genders are quite balanced
- Marital status is also balanced
- Most customers have highschool level education. There are only 1,8% graduate students. We could join both categories ('2' and '3') in a category called university & over
- Both the 'Occupation' and 'Settlement size' features seem to have a larger enough quantity of instances of each category.

![plot3](https://github.com/micheldc55/customer_segmentation_project/blob/main/Education.png)

From the education feature plot above, we notice that there are only 1,8% graduate students (students with graduate level education). In a DataFrame that has only 2000 entries, this means that there are only 36 people with graduate studies in the dataset. So few instances may lead to worse results in the model, so we are joining them 
