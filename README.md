## Project Proposal
### Introduction/Background
Corona-virus disease 2019[3] is an infectious disease caused by severe acute respiratory syndrome corona-virus 2 and as of September 2020 has infected more than 31 million people worldwide. With six months past the day when the World Health Organization declared COVID-19 a pandemic and the fear of a second wave buzzing, it becomes important to analyze the effectiveness different responses to the virus in order to ensure we are making the right decisions and polices moving forward.

### Problem Definition
Given the health and lock down data[2] available regarding COVID-19, predict the total number of cases a country is to experience over the duration of the pandemic.

### Data Collection

Our data is obtained from the 'Our World in Data' dataset from Oxford's COVID-19 Government Response Tracker (available on GitHub). This dataset is a representation of the attributes of various countries that have gone through COVID-19 cases. Each country measures contains 8 total features, all of which are combined by a per-determined formula to determine the Government Stringency Index, or the severity of the COVID-19 spread in the particular country. These features are as follows: Population, Population Density, Median Age (of all individuals in country), Aged 65 or older (Number of people in the country), GDP per capita, Cardiovascular Death Rate, Diabetes Prevalence, and Human Development Index. To clean this data in an optimal manner, we perfomed the following pre-processing and feature-selection techniques:

Space for Data Cleaning Group to talk about their approaches. As per the directions on the website: Make sure to talk about the features and different feature selection approaches.

### Methods

#### Unsupervised Learning 

Our goal in the Unsupervised Learning process is to cluster countries based on attributes such as size, population, density, and other relevant features. This clustering is done in order to make sensible comparisons; due to the varying conditions of each country, there is not likely to be a singular prediction mechanism for the number of COVID-19 cases that countries are going to experience. Rather, due to the wildly differing situations unique to each country, it makes sense that there could exist many Supervised Learning solutions (in terms of different models, different parameters of a single model) for different sets of countries, based on their individual situations. As such, our goal is to separate the cleaned country data into different clusters via Unsupervised Learning, upon which Supervised Learning techniques will be applied for each individual cluster. 

To group similar countries together, we used the KMeans clustering technique, that has an underlying assumption of circular clustering of data. Principal Component Analysis (PCA) was also used in this algorithm to reduce the number of features for each country to produce optimal visualizations. Finally, the generated KMeans models were evaluated using common metrics and scores such as the Elbow Method and the Silhouette Coefficient. To implement most of the Unsupervised Learning process, the Python Sci-Kit Learning package was used (in conjunction with the NumPy and Pandas libraries). In addition, for all techniques, we first standardized the data using StandardScaler() to ensure that features with large numerical values did not disproportionately contribute to the clustering. 

First, we applied the KMeans algorithm in the Standard way, by considering all dataset features for every country in computing the centroids. We used the Elbow Method in this instance to deterime the optimal number of clusters needed in this case. From this analysis, we concluded that using 5 clusters led to KMeans obtaining the best results. For this approach, we generated plots of the number of clusters versus inertia value (sum of squared distances of samples to their closest cluster center), as well as the corresponding cluster assignment for the best (5) cluster number (using PCA for dimensionality reduction). These visualizations are shown below:

![Elbow Method for Optimal Number of Clusters](clusteringImages/image1.png)

![Clustering Distribution for ideal 5 Clusters](clusteringImages/image2.png)

In addition, we also decided to apply dimensionality reduction on our data, and see if the performance of the KMeans algorithm on the resulting feature-reduced subset increased. This feature reduction was performed in 2 ways. The first method we employed was simply to consider all possible subsets of all features in our data, for every datapoint. Since there were 8 total features, this meant choosing all combinations of 1 feature, all combinations of 2 features, all combinations of 3 features, and so on. We used the itertools package in Python to generate a combination list (in terms of feature indices) of all possible features. 

For each combination, we sliced our data to consider only these features. We then applied the KMeans algorithm on this reduced dataset, varying the number of clusters from 2 to 50. We then obtained the assignment of the final clustering, for the specific feature split and cluster number. In addition, to measure quality of the clustering in a more empirical way, we also measured the goodness of the clustering by computing the Silhouette Score of the final assignment. This process was repeated for every total number of features (1 to 8), all possible subsets within every feature number, and all possible number of clusters (2 to 50).

Finally, within each total number of features (1 to 8), we identified the feature subset that produced the clustering with the maximum silhouette score (for every cluster number from 2 to 50). We plotted these results on a graph, where the x-axis represented the number of clusters, and the y-axis represented the silhouette score. On this graph, data points were considered one of eight colors, to indicate the total number of features of the max clustering. This graph is shown below:

![Number of Clusters v. Max Silhouette Score (for all number of features)](clusteringImages/image3.png)

From the graph, it was clear that considering only 1 feature produced clusterings with the maximum silhouette score, for every number of clusters. As such, within all clusterings that were generated from 1 feature (there were only 392 such clusterings, one for each individual feature and number of clusters), we determined the feature and number of clusters that produced the clustering with the maximum silhouette score was 'Population' and 2 clusters, respectively (with a silhouette score of 0.9691749073425401). For this clustering ('Population' and 2 clusters), we obtained the corresponding clustering assignment on the entire dataset (with all features). A visualization of this assignment is shown below:

![Clustering Distribution for 'Population', 2 Clusters](clusteringImages/image4.png)

The second method that we applied was using PCA to reduce the number of features. Instead of considering all possible subsets for every feature number (1 to 8), for each feature number, we considered a single dataset that retained the maximum variance of the original data, which we accomplished via Principal Component Analysis. In this case, for every feature number (from 1 to 8), we used PCA to reduce the number of features to that feature number, and then applied the KMeans algorithm on this reduced dataset as before, varying the number of clusters from 2 to 50. We also used the Silhouette Score to evaluate the goodness of the clustering for every assignment.

We then created a similar graph as before, where we varied the number of clusters from 2 to 50 on the X-axis and the Silhouette Score (no need for max Silhouette Score, since there was only one clustering per number of features and cluster number) on the Y-axis, color-coding the data points to indicate the number of reduced features of the data. We noticed that, irrespective of the number of reduced features, the Silhouette Score of the clustering assignment remained essentially the same (up to the thousandths place), for all number of clusters. As such, there is only one color on the visualization below (due to the considerable overlap between all feature numbers):

![Number of Clusters v. Silhouette Score (for all number of reduced features)](clusteringImages/image5.png)

Using NumPy functions, we then obtained the (number of reduced features, number of clusters) pair that produced the clustering with the maximum Silhouette Score. The resulting pair that was obtained was (1 reduced feature, 1 cluster), which produced a maximum Silhouette score of 0.9691749073272797). For this clustering (1 reduced feature and 2 clusters), we generated the corresponding cluster assignment on the entire dataset (with all features). A visualization of this assignment is shown below.

![Clustering Distribution for 1 Reduced Feature, 2 Clusters](clusteringImages/image6.png)

In conclusion, we see that, from our three feature selection approaches, considering all features (approach 1) produced a clustering assignment with 5 clusters, and performing dimensionality reduction (feature subsets and PCA, approaches 2 and 3) produced identical clustering assignments (with marginal silhouette score difference) with 2 clusters. From the visualizations, it is clear that the featured reduced clusterings are more optimal, in terms of both the maximum Silhouette Score as well as the distances of the clusters from each other. As such, based on this ideal assignment, we will partition our countries into 2 datasets and apply separate Linear Regression models to each dataset. These techniques are discussed more in the Supervised Learning Section.

#### Supervised Learning 

Within each cluster, we will see which countries fared relatively better. We will pay special attention to their lockdown policy - how stringent it was, when it was enacted, and the situation of the pandemic at the introduction of these measures. By doing so, we will be able to a build regression models to to better suggest a social policy for a new country that is similar to the cluster that could help reduce the effects of this virus.

Given enough time, we would also like to examine how to formulate the best policy to both protect lives and the economy. Within each cluster, we will look at the GDP of the countries and how it fared as the pandemic took hold. We are particularly interested in seeing how the GDP reacted to public policy (if any), and how it fluctuated with the number of cases. We can leverage this new model with the regression model project number of cases and deaths to strike a balance between halting the spread of pandemic with the least amount of economic effect. 

### Results
Potential results include achieving a high accuracy predictive model for countries still facing the pandemic, allowing for the variation of the Government Stringency Index[1] to in order to understand how effective differing lock downs were, and developing a second model to predict GDP loss in order to find the happy medium between lives saved and GDP losses.

### Discussion
By having a model to predict total number of cases, one becomes able to isolate individual features, under the assumption of independent, so that analysis of government pandemic policies may better prepare the world for future infectious diseases. 

### References
[1] Hale, Webster, Petherick, Phillips, and Kira (2020). Oxford COVID-19 Government Response Tracker - Last updated 28 September, 19:30 (London Time)

[2] “Owid/Covid-19-Data.” GitHub, 21 Apr. 2020, github.com/owid/covid-19-data.

[3] Roser, Max, and Hannah Ritchie. "Coronavirus Disease (COVID-19)." Our World in Data, 4 Mar. 2020, ourworldindata.org/coronavirus.
