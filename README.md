## Project Proposal
### Introduction/Background
Corona-virus disease 2019[3] is an infectious disease caused by severe acute respiratory syndrome corona-virus 2 and as of September 2020 has infected more than 31 million people worldwide. With six months past the day when the World Health Organization declared COVID-19 a pandemic and the fear of a second wave buzzing, it becomes important to analyze the effectiveness of different responses to the virus in order to ensure we are making the right decisions and polices moving forward.

### Problem Definition
Given the health and lock down data[2] available regarding COVID-19, predict the total number of cases a country is to experience over the duration of the pandemic.

### Data Collection

Our data is obtained from the 'Our World in Data' dataset from Oxford's COVID-19 Government Response Tracker (available on GitHub). This dataset is a representation of the attributes of various countries that have gone through COVID-19 cases. Each country measures contains 8 total features, all of which are combined by a per-determined formula to determine the Government Stringency Index, or the severity of the COVID-19 spread in the particular country. These features are as follows: Population, Population Density, Median Age (of all individuals in country), Aged 65 or older (Number of people in the country), GDP per capita, Cardiovascular Death Rate, Diabetes Prevalence, and Human Development Index. To clean this data in an optimal manner, we performed the following pre-processing and feature-selection techniques:

The data set that we obtained wasn’t perfect. For certain countries, there were missing values for key information like number of tests or median age. For the countries that were missing significant amounts of data, we simply removed them from the data set because rather than having to deal with null values or mapping them to estimated values, we deemed that it is better to operate with complete data. Therefore, although our original data started out with 209 countries, after deleting the countries with significant amounts of blank data, we were left with 156 countries. 

In addition, we wanted our data to be all numerical (no strings or phrases). This is very important in our plans to build regression models. For this component we had to change up the ‘date’ and ‘country’ columns; the ‘date’ column was in date format and the country column was in string format. Therefore, we decide that for the date we would simply have a ‘date counter’ from the first day of data signifying day 1 and so forth. Everyday after that would be subsequently incremented. An issue we ran into was that not all countries had the same start date; some countries’ data began in January whilst others only started reporting in April. We determined (through thorough examination of the data) that the latest start date across all countries was April 1st, so to ensure uniformity we set that as day ‘1’, and removed any data from dates before April 1st. Now, all of our data ranges from April 1st to October 21st. 

In terms of the countries column, similar to the modifications made in the date column, we instated a mapping for the countries from 1 to 156 for what each country numerically represents. This was done in alphabetical order starting with Afghanistan.  

In terms of selecting key features, there are certain features that intuitively have more of an impact on the government stringency index than others. For example, information about population density is more important in determining how much to shut down a country than the continent of a specific country. Also, there were certain features that seemed to have a great deal of overlap with each other. Rather than incorporating all those features (which may have resulted in data/model skewing), we selected only one of the features in that overlap. An example of this is the ‘aged_65_older’ and ‘aged_70_older’ columns; ‘aged_65_older’ contains information about ‘aged_70_older’ within, so we decided to just include the ‘aged_65_older’ column. 

There are certain improvements that can be made. In terms of the country, rather than have a numerical value from 1 to 156, we could change this to be one-hot encoded. This would be useful in a regression model to prevent the model from attempting to recognize faulty correlations. Overall, we could continue testing between different columns and experimenting to see if there is a strong correlation between any two variables. We can do this analysis and determine if incorporating or removing certain features serves to improve our models.


### Methods

#### Unsupervised Learning 

Our goal in the Unsupervised Learning process is to cluster countries based on attributes such as size, population, density, and other relevant features. This clustering is done in order to make sensible comparisons; due to the varying conditions of each country, there is not likely to be a singular prediction mechanism for the number of COVID-19 cases that countries are going to experience. Rather, due to the wildly differing situations unique to each country, it makes sense that there could exist many Supervised Learning solutions (in terms of different models, different parameters of a single model) for different sets of countries, based on their individual situations. As such, our goal is to separate the cleaned country data into different clusters via Unsupervised Learning, upon which Supervised Learning techniques will be applied for each individual cluster. Below is a world map showing the results of our clustering algorithm. Countries excluded due to missing data are colored in light grey.   

![Countries clustered together by similar conditions](clusteringImages/country_cluster.png)

Our Unsupervised Learning Approach categorizes clusters by similarity, using both a Health Profile and a Resource Profile for each individual country as a similarity metric, in addition to all of the data obtained in the Data Collection and Cleaning phase. The Health Profile for each country contains an age-standardized prevalence of high-risk diseases in each country (which helps to gauge the severing of COVID-19 cases in that country) obtained from the Global Health Data Exchange (GHDX), such as Cardiovascular Disease, Type 2 Diabetes, Cancer, COPD, and Chronic Kidney Diseases. The resource profile for each country incorporates the economics of each country into account with respect to its ability to control the spread of the COVID-19 epidemic, and includes attributes such as the Human Development Index (HDI), the Per-capita Gross Domestic Product (GDP), and the Universal Health Coverage (UHC) service index (provided by WHO). As we will see below, using these additional features helped to improve the overall accuracy of our country clusters.

Next, to group similar countries together, we used the KMeans clustering technique, that has an underlying assumption of circular clustering of data. Principal Component Analysis (PCA) was also used in this algorithm to reduce the number of features for each country to produce optimal visualizations. Finally, the generated KMeans models were evaluated using common metrics and scores such as the Elbow Method, the Distortion Method, the Silhouette Coefficient, and the Davies-Bouldin Index. To implement most of the Unsupervised Learning process, the Python Sci-Kit Learning package was used (in conjunction with the NumPy and Pandas libraries). In addition, for all techniques, we first standardized the data using StandardScaler() to ensure that features with large numerical values did not disproportionately contribute to the clustering. 

First, we applied the KMeans algorithm in the Standard way, by considering all dataset features for every country in computing the centroids. We varied the number of clusters from 2 to 50, and obtained the resulting country assignment for each cluster number. We then evaluated these clustering using the Elbow Method (number of clusters versus ineria value), the Distortion Method (number of clusters versus sum of squared distances of samples to their closest cluster center), the Silhouette Score (a measure of how similar an object is to its own cluster compared to other clusters), and the Davies-Bouldin Index (ratio between the cluster's scatter and cluster's separation). For each internal evaluation metric, we determined the number of clusters that we optimal. These results are displayed below.

First, using the Elbow Method, we concluded that using 5 clusters led to KMeans obtaining the best results. For this approach, we generated plots of the number of clusters versus inertia value, as well as the corresponding cluster assignment for the best (5) cluster number (using PCA for dimensionality reduction). These visualizations are shown below:

![Elbow Method for Optimal Number of Clusters](clusteringImages/image10.png)

![Clustering Distribution for ideal 5 Clusters](clusteringImages/image2.png)

Next, using the Distortion Method, we concluded that using 7 clusters led to KMeans obtaining the best results. For this approach, we generated plots of the number of clusters versus the Distortion Score. This visualization is shown below:

![Distortion Method for Optimal Number of Clusters](clusteringImages/image9.png)

Then, using the Silhouette Score, we concluded that using 4 clusters led to KMeans obtaining the best results, with a Silhouette Score of around 0.3. For this approach, we generated plots of the number of clusters versus the Silhouette Coefficient. This visualization is shown below:

![Silhouette Score for Optimal Number of Clusters](clusteringImages/image7.png)

Finally, using the Davies-Bouldin Index, we concluded that using 11 clusters led to KMeans obtaining the best results. For this approach, we generated plots of the number of clusters versus the Davies-Bouldin Index. This visualization is shown below:

![Davies-Bouldin Index for Optimal Number of Clusters](clusteringImages/image8.png)

To complete the evaluation of the country clustering when all features were considered, we took each clustering deemed optimal by the four evaluation metrics above, and ran our Supervised Learning algorithms (discussed more in the next section) to see how well we would be able to predict the number of new deaths for countries part of our test dataset. We found that the clustering that resulting in the lowest Mean Absolute Error prediction (in terms of deviation from the real number of new deaths) was the 5 clustering set obtained from the Elbow Method evaluation metric. Therefore, we decided that the Elbow Method assignment with 5 clusters was the most optimal clustering obtained when all data features were considered. The quantitative performance obtained using this clustering is shown below:

![Mean Absolute Error for the 5 models obtained via Elbow-Method Clustering](clusteringImages/silhouette_supervised/image3.PNG)

In addition, we also decided to apply dimensionality reduction on our data, and see if the performance of the KMeans algorithm on the resulting feature-reduced subset increased. This feature reduction was performed in 2 ways. The first method we employed was simply to consider all possible subsets of all features in our data, for every data point. Since there were 8 total features, this meant choosing all combinations of 1 feature, all combinations of 2 features, all combinations of 3 features, and so on. We used the itertools package in Python to generate a combination list (in terms of feature indices) of all possible features. 

For each combination, we sliced our data to consider only these features. We then applied the KMeans algorithm on this reduced dataset, varying the number of clusters from 2 to 50. We then obtained the assignment of the final clustering, for the specific feature split and cluster number. In addition, to measure quality of the clustering in a more empirical way, we also measured the goodness of the clustering by computing the Silhouette Score of the final assignment. This process was repeated for every total number of features (1 to 8), all possible subsets within every feature number, and all possible number of clusters (2 to 50).

Finally, within each total number of features (1 to 8), we identified the feature subset that produced the clustering with the maximum silhouette score (for every cluster number from 2 to 50). We plotted these results on a graph, where the x-axis represented the number of clusters, and the y-axis represented the silhouette score. On this graph, data points were considered one of eight colors, to indicate the total number of features of the max clustering. This graph is shown below:

![Number of Clusters v. Max Silhouette Score (for all number of features)](clusteringImages/image3.png)

From the graph, it was clear that considering only 1 feature produced clusterings with the maximum silhouette score, for every number of clusters. As such, within all clusterings that were generated from 1 feature (there were only 392 such clusterings, one for each individual feature and number of clusters), we determined the feature and number of clusters that produced the clustering with the maximum silhouette score was 'Population' and 2 clusters, respectively (with a silhouette score of 0.9691749073425401). For this clustering ('Population' and 2 clusters), we obtained the corresponding clustering assignment on the entire dataset (with all features). A visualization of this assignment is shown below:

![Clustering Distribution for 'Population', 2 Clusters](clusteringImages/image4.png)

The second method that we applied was using PCA to reduce the number of features. Instead of considering all possible subsets for every feature number (1 to 8), for each feature number, we considered a single dataset that retained the maximum variance of the original data, which we accomplished via Principal Component Analysis. In this case, for every feature number (from 1 to 8), we used PCA to reduce the number of features to that feature number, and then applied the KMeans algorithm on this reduced dataset as before, varying the number of clusters from 2 to 50. We also used the Silhouette Score to evaluate the goodness of the clustering for every assignment.

We then created a similar graph as before, where we varied the number of clusters from 2 to 50 on the X-axis and the Silhouette Score (no need for max Silhouette Score, since there was only one clustering per number of features and cluster number) on the Y-axis, color-coding the data points to indicate the number of reduced features of the data. We noticed that, irrespective of the number of reduced features, the Silhouette Score of the clustering assignment remained essentially the same (up to the thousandths place), for all number of clusters. As such, there is only one color on the visualization below (due to the considerable overlap between all feature numbers):

![Number of Clusters v. Silhouette Score (for all number of reduced features)](clusteringImages/image5.png)

Using NumPy functions, we then obtained the (number of reduced features, number of clusters) pair that produced the clustering with the maximum Silhouette Score. The resulting pair that was obtained was (1 reduced feature, 2 clusters), which produced a maximum Silhouette score of 0.9691749073272797). For this clustering (1 reduced feature and 2 clusters), we generated the corresponding cluster assignment on the entire dataset (with all features). A visualization of this assignment is shown below.

![Clustering Distribution for 1 Reduced Feature, 2 Clusters](clusteringImages/image6.png)

In conclusion, we see that, from our three feature selection approaches, considering all features (approach 1) produced a clustering assignment with 5 clusters via the Elbow Method, and performing dimensionality reduction (feature subsets and PCA, approaches 2 and 3) produced identical clustering assignments (with marginal silhouette score difference) with 2 clusters. From the visualizations, it is clear that the featured reduced clusterings are more optimal, in terms of both the maximum Silhouette Score as well as the distances of the clusters from each other. However, 2 clusterings is too low to generate a Supervised Learning model. As such, we make the decision to use the second-most ideal assignment of 5 clusters. We will partition our countries into 5 datasets, based on the first clustering shown in the World Map, and apply separate Linear Regression models to each dataset. These techniques are discussed more in the Supervised Learning Section.

#### Supervised Learning 

Our goal in the Supervised Learning process is to develop a regression model to predict new deaths from COVID-19. Our development of this model begins with a visual and statistical analysis of our feature set. By better understanding the statistical variation of each feature, and the correlation between features, we set ourselves up for a more informed understanding of the domain prior to model training. This analysis is included below:

What we are trying to predict:

![Country 27 Days since 4/1/2020 vs New Deaths](TrainingImages/Country27Trend.png)

![Country 75 Days since 4/1/2020 vs New Deaths](TrainingImages/Country75Trend.png)

![Country 150 Days since 4/1/2020 vs New Deaths](TrainingImages/Country150Trend.png)

We split our data with a 70/15/15 split for training, testing, and validation respectively. This yields 110 countries for training, and 23 countries for both testing and validation. 

A statistical analysis of our training data:

||count|mean|std|min|25%|50%|75%|max|
|---|---|---|---|---|---|---|---|---|
|Country (label)|26762.0|1.047587e+02|5.967297e+01|1.000|54.000|1.060000e+02|1.530000e+02|2.090000e+02|
|Date (label)|26762.0|1.023138e+02|5.898397e+01|1.000|51.000|1.020000e+02|1.530000e+02|2.040000e+02|
|new_cases|26762.0|1.194505e+03|5.748082e+03|-8261.000|3.000|5.700000e+01|4.320000e+02|9.757000e+04|
|new_deaths|26762.0|3.261430e+01|1.525499e+02|-1918.000|0.000|1.000000e+00|7.000000e+00|4.928000e+03|
|new_tests|26762.0|1.420623e+04|7.878015e+04|-3743.000|0.000|4.100000e+01|4.321750e+03|1.492409e+06|
|stringency_index|26762.0|5.960808e+01|2.610358e+01|0.000|43.520|6.528000e+01|8.009000e+01|1.000000e+02|
|population|26762.0|4.708915e+07|1.625562e+08|97928.000|3280815.000|1.009927e+07|3.481387e+07|1.439324e+09|
|population_density|26762.0|2.058378e+02|6.553158e+02|1.980|35.608|8.134700e+01|1.975190e+02|7.915731e+03|
|median_age|26762.0|3.086748e+01|8.990814e+00|15.100|23.100|3.060000e+01|3.910000e+01|4.820000e+01|
|aged_65_older|26762.0|8.973122e+00|6.250375e+00|1.144|3.552|6.769000e+00|1.443100e+01|2.704900e+01|
|gdp_per_capita|26762.0|1.941902e+04|1.962766e+04|661.240|5189.972|1.325495e+04|2.771785e+04|1.169356e+05|
|cardiovascular_death_rate|26762.0|2.549043e+02|1.177259e+02|79.370|164.905|2.412190e+02|3.226880e+02|7.244170e+02|
|diabetes_prevalence|26762.0|7.741321e+00|3.851030e+00|0.990|5.180|7.110000e+00|1.008000e+01|2.202000e+01|
|human_development_index|26762.0|7.196080e-01|1.506658e-01|0.354|0.601|7.510000e-01|8.430000e-01|9.530000e-01|

Following a statistical analysis, we plot a few of the features against our training label to see if any visual correlations could be identified. As expected, new_deaths is strongly correlated to new_cases and new_tests, while other features such as stringency_index and population_density do not display as obvious of a correlation. 

![Cases KDE](VisualAnalysisImages/Date_cases_deaths_kde.png)

![Government KDE](VisualAnalysisImages/deaths_government_kde.png)

Following a visual analysis of our data, each feature is normalized. Then, we trained a linear regression model and a DNN model. Our linear regression model was trained with an Adam optimizer, using a learning rate of 0.1, 100 epochs, a 20% validation split, and was evaluated using absolute mean error. Total train time took 26 seconds.

Finally, we trained a DNN with an Adam optimizer, using a learning rate of 0.001, 100 epochs, a 20% validation split, and was evaluated using absolute mean error. Total train time took 35 seconds. This model includes 6 layers, our features, 3 layers of 64 neurons, and an output layer


### Results
First off, let us examine the loss functions for each model on the training data. Notice how the validation data loss does not decrease with each epoch. This demonstrates an over fitting to the training data. Graphs depicted below:

![Linear Training Loss](ResultsImages/LinearLoss.png)

![DNN Training Loss](ResultsImages/DNNLoss.png)

Next we examine the mean absolute error of the testing data. Notice how the error is much smaller for our testing data than it was for our training data. This should be looked into further for the development of a more general model.

|Model|Mean absolute error [new_deaths]|
|---|---|
|linear_model|8.275075|
|dnn_model|8.240737|

Finally, let us examine the difference between predicted and real results. First this is done by graphing predicted vs real results. Both models demonstrate a direct linear relationship between predicted and real values. Next let us construct a histogram delineating the difference between real and predicted values. From this diagram it is clear the DNN has a tendency to under predict the number of deaths. Finally, allow us to visualize a few predictions of the test data in direct comparison to the real values as plotted against time.

|Linear Regression|DNN|
|---|---|
|![Linear True Vs Predicted](ResultsImages/LinearTrueVsPredicted.png)|![DNN True Vs Predicted](ResultsImages/DNNTrueVsPredicted.png)|
|![Linear Prediction Error](ResultsImages/LinearPredictionError.png)|![DNN Prediction Error](ResultsImages/DNNPredictionError.png)|
|![Linear 22 Prediction](ResultsImages/Linear22Prediction.png)|![DNN 22 Prediction](ResultsImages/DNN22Prediction.png)|
|![Linear 39 Prediction](ResultsImages/Linear39Prediction.png)|![DNN 39 Prediction](ResultsImages/DNN39Prediction.png)|
|![Linear 118 Prediction](ResultsImages/Linear118Prediction.png)|![DNN 118 Prediction](ResultsImages/DNN118Prediction.png)|

### Discussion
After an observation of the predicted vs real results, it becomes quite clear that the model is thrown off by the noise in data. New deaths spike on various days depending on the country and how deaths are reported. Applying a uniform or gaussian smoothing to our dataset would serve to yield a more consistent and generalized model. 

Another point for improvement is our data currently contains a few negative values for new_cases, new_tests, and new_deaths. We are unsure what this represents within the data and thus have left these values in for now. This could be why our current model sometimes predicts people coming back to life as represented by negative deaths.

Some of our countries included never experienced a death from COVID-19. A discussion was held to determine if these countries should be included, as they could provide insight in to a more general model, but at the same time could be lowering the accuracy for countries that do experience death from COVID-19. We ultimately decided to remove these countries, as the advantage of improved accuracy far outweighted the tradeoff of model insight.

As of current, all of our data begins on April 1st 2020. This decision was made to clean the data, but could be limiting our model in the prediction of a virus's lifetime. Perhaps a more generalized model would begin each country's timeline when the first case was discovered. 

### Conclusions 
Some successes that we had on this COVID-19 Machine Learning project are as follows. We were able to clean the noisy and NaN values present in our original dataset to remove those countries containing large amounts of invalid or missing data. Additionally, to a certain degree of optimization, we determined the number of clusters (along with the relevant features) that would partition our country set into valid subgroups, and developed Supervised Learning Models - Linear Regression and Deep Neural Network (DNN) - to accurately predict new COVID-19 related deaths for various countries.

Additionally, there are some extensions on this project that we can make if we were to pursue it in the future. One possible addition would be to incorporate the economics of the countries in our dataset, and use the resulting financial conclusions to predict the impact of COVID-19 on country economy (in addition to predicting new deaths). Another possible addition would be to further clean our dataset using different smoothing techniques, and continue to optimize the performance of our models by varying the specific features and cluster assignments used in the prediction algorithms. 

Overall, our group enjoyed working on this project over the course of the semester, and learned a lot about how to apply Unsupervised and Supervised Machine Learning Algorithms in a practical setting.

### References
[1] Hale, Webster, Petherick, Phillips, and Kira (2020). Oxford COVID-19 Government Response Tracker - Last updated 28 September, 19:30 (London Time)

[2] “Owid/Covid-19-Data.” GitHub, 21 Apr. 2020, github.com/owid/covid-19-data.

[3] Roser, Max, and Hannah Ritchie. "Coronavirus Disease (COVID-19)." Our World in Data, 4 Mar. 2020, ourworldindata.org/coronavirus.
