# Customer Segmentation Project

## Project Summary

In this project I create a customer segmentation model for Data obtained from Kaggle. Within the project I explore the features, visualize them, apply normalizations, review the data, create clusters, evaluate them and apply business-intelligence to the results.

The purpose of this project was to explore customer segmentation tools through clustering, using evaluation metrics and customer understanding through visualization to create insightful groups of customers that could be exploited in online marketing processes or similar tasks that require your customers to be grouped by their features.

<p align = 'center'>
  <img width="550" height="300" src="https://github.com/micheldc55/customer_segmentation_project/blob/main/classes_plot_PCA.png">
</p>

The above image shows the results of the clustering algorithm applied to 6 clusters. The conclusions about the clusters will be discussed below in the **Results** section. To briefly comment about what you can see in the image, the results show that the projection of the 6 clusters into a 3-dimensional space (using PCA) creates groups that are very well separated. 

This is a tool often used to visualize your clusters when the number of features is greater than 3. You reduce your space to two or three new features that are a linear combination of your original features. It allows the user to visualize the groups projected into these new features. You loose some information in the process, but if the information loss is not significant, you gain a powerful visualization tool for the clusters you create.

# Table of Contents:

- [Project Objective](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#project-objective)
  - [Libraries used](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#libraries-used)
- [Process Overview](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#process-overview)
- [Detailed Process](https://github.com/micheldc55/customer_segmentation_project/new/main?readme=1#detailed-process)
- []()


## Project Objective

The objective of this project was to use an unknown dataset and apply insights gained from the Data itself and the clustering models to obtain customer groupings that would provide interesting shared characteristics within each group that could be exploited. I believe the most captivating part about this project was the evaluation and interpretation process, as it challenges not only the methods used, but also the understanding of the data.

### Libraries used

For this project, I used the libraries detailed below. On the left side of the table you can see the Python library used, and on the right column under 'Purpose' you can see for what it was used for.

| Library/Tool | Purpose |
| :----------: | :-----: |
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
| Graphviz | Decision Tree Visualization |

After conducting the necessary imports, we begin working with the data by importing the dataset using pandas. The file is a .csv containing information about the customers' age, income, gender, marital status, education level, occupation, and size of city they live in.

## Process Overview

During the process of obtaining the customer groupings (or clusters) I went through a step-by-step process which allowed me to go from importing the data to finally interpreting the clusters in a continuous process. This doesn't mean that this is the only way to conduct a customer segmentation process, but the steps taken here are a good way to organize your process if you haven't done any clustering or segmentation before. 

### Steps taken through the process:

The process is shown in greater detail in the notebook within this repository. Below there's an outline of the steps taken in the process, but I do encourage you to check them out in the notebook, where there is much more information avaliable.

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
10) Interpretation of results (clusters)

### Results

After carefull analysis of the data we reach some conclusions that were expected to come up when the clusters were determined. This are relationships we can see a priori within the data, such as: "People with Occupation = '0' (unemployed) tend to live in small cities within our customers". A lot of these relationships were found during the Exploratory Data Analysis and were used as a way to quickly assess the accuracy of the model. If you are interested in seeing this a priori results, they are included below.

To determine the number of clusters to use, two metrics were combined: 'WSS' (Within-Sum-of-Squares) and 'Silhouette' scores. The two metrics were used to contrast the results of each method and to check if they were coherent. As you can see in the notebook, the actual result is  over 19 clusters. But there is a spike in the Silhouette scores at 6-7 clusters. If we consider this situation for a moment, there are 2000 customers in this dataset, so it is quite small. If we create more than 19 clusters with this data, we are probably not going to be feeding the model enough significant information within the training phase, and they won't perform well. Moreover, given the low quantity of features the model has, it will be easier to interpret the clusters if there are fewer of them.

Once this was determined, another interesting process was the interpretation aspect of the clusters. Clustering algorithms belong to the unsupervised machine learning classification, and as such there are no labels to identify which class does each point belong to. So interpretation in this type of problems can be tricky. To solve this in the problem, three methods were contrasted to see if all of them reached the same conslusions. 

These methods were:

### 1) Building a Decision Tree Classifier around the now labeled data

This is one of the preferred methods of mine when trying to interpret the results of a clustering algorithm. You build a Decision Tree model, which selects the best feature to split at and what is the best value to split at by using information gain and entropy/Gini. This means that using this method on the classes obtained by the clustering model will give us an insight on what are the features that give us most of the 'intel' on what features move the needle the most, granted that the Decision Tree is accurate at predicting the classes.

Below you can see the result of applying this algorithm as an interpretation tool for the problem. As you will see in the image, the tree is composed of the variables in our problem. 

<p align = 'center'>
  <img width="1000" height="450" src="https://github.com/micheldc55/customer_segmentation_project/blob/main/decision tree.png">
</p>

Each arrow on the left of each box represents the case where the condition at the top of the box is True, and the arrow on the right means the condition is false. You can see that the most 'valuable' feature to classify information is 'Martial Status', or whether the customer is married or not, followed by 'Settlement size' and 'Sex'. So if we had to pick only three features to consider, these would do the trick.

#### A summary of the results is shown below:

| **Cluster Number** | Cluster Description |
| :------------: | :-----------------: |
| **Cluster 0** | Single + Small city + Male + Employed (employee/self-employed) |
| **Cluster 1** | Non-single + Female + small city + unemployed or employees |
| **Cluster 2** | Non-single + Female + medium/large city + management/self-employed |
| **Cluster 3** | Single + Medium/big city + Male + Employed (employee or self-employed) |
| **Cluster 4** | Non-single + Males |
| **Cluster 5** | Single + Small city + Female |

### 2) Using centroid values as representative features of the cluster

The centroids give us a general idea of what the cluster represents. Below are the results of looking at the clusters' centroids and assigning the class that best fits the value in each; some features are not shown because they were determined to be non-significant in determining clusters, as shown in the notebook. The main disadvantage of using cluster centroids is that they only are representative in clusters with high compactness. The centroids are very sensible to points that are very disperse or have outliers.

| Cluster Number | Sex |	Marital status |	Education |	Occupation |	Settlement size |
| :------------: | :----: | :----------: | :--------: | :--------: | :--------------: |
| C0 |	Male |	Single |	No Education |	Employed |	Medium City |
| C1 |	Female |	Married |	High-school or above |	Unemployed |	Small City |
| C2 |	Female |	Married |	Highly Educated |	Employed |	Medium City |
| C3 |	Male |	Single |	Highly Educated |	Managerial/Business Owner |	Big City |
| C4 |	Male |	Married |	High-school or above |	Employee |	Small City |
| C5 |	Female |	Single |	No Education |	Unemployed |	Medium City |

### 3) Plotting the features' distributions/countplots for each cluster

The third method is, in my opinion, the hardest to interpret but it's also the only one were you take all data into account when making a decision. This is simply isolating each cluster on its own and running summary statistics on each one. This can be done here because the classes don't have too many possible outcomes and there is a small number of features, the complexity of interpretation raises with the number of features and number of classes in each variable.
![multiplot_cluster4_example](https://github.com/micheldc55/customer_segmentation_project/blob/main/cluster4_results.png)
From this example we can deduce that this cluster consists of only females (Sex=1 represent females), who live in medium/big cities and are married and employed. So we can deduce from the picture alone that this is plots are taken from cluster 2. 

## Conclusions

All three methods show similar results when dealing with out data, so this is a good sign. We kept the results from the Decision Tree, since they were the simplest to understand from a business point of view. They would be the easier to convey to a person who doesn't know about Machine Learning.

We can establish an even easier relationship between clusters and the customer they represent by giving them names, as you can see below. The names are just to give some easy to remember representation, they mean no harm of course and may be a little different from what the actual cluster represents.

**Cluster 0:** 'The Overcomers'.        Single males who aren't educated but have a job in a medium sized city.
**Cluster 1:** 'The Moms'.              Married females with no jobs or employees in small cities.
**Cluster 2:** 'Women empowered'.       Married females with high educations and high income jobs, who live in medium/large cities
**Cluster 3:** 'The Hotshots'.          Single males with high education and high income jobs from big cities.
**Cluster 4:** 'Taken men'.             Married males
**Cluster 5:** 'The small town Janes'.  Single females from small cities

If the marketing team now had a male, married customer, they would immediatly assign him to **cluster 4**. 
