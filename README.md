## Project Proposal
### Introduction/Background
Corona-virus disease 2019 is an infectious disease caused by severe acute respiratory syndrome corona-virus 2 and as of September 2020 has infected more than 31 million people worldwide. With six months past the day when the World Health Organization declared COVID-19 a pandemic and the fear of a second wave buzzing, it becomes important to analyze the effectiveness different responses to the virus in order to ensure we are making the right decisions and polices moving forward.

### Problem Definition
Given the health and lock down data available regarding COVID-19, predict the total number of cases a country is to experience over the duration of the pandemic.

### Methods
We begin by clustering countries based on attributes such as size, population, density, and other relevant features in order to make sensible comparisons - it is highly unlikely there exists a one-size-fits-all solution due to the wildly differing situations unique to each country. 

Within each cluster, we will see which countries fared relatively better. We will pay special attention to their lockdown policy - how stringent it was, when it was enacted, and the situation of the pandemic at the introduction of these measures. By doing so, we will be able to a build regression models to to better suggest a social policy for a new country that is similar to the cluster that could help reduce the effects of this virus.

Given enough time, we would also like to examine how to formulate the best policy to both protect lives and the economy. Within each cluster, we will look at the GDP of the countries and how it fared as the pandemic took hold. We are particularly interested in seeing how the GDP reacted to public policy (if any), and how it fluctuated with the number of cases. We can leverage this new model with the regression model project number of cases and deaths to strike a balance between halting the spread of pandemic with the least amount of economic effect. 

### Potential Results
Potential results include achieving a high accuracy predictive model for countries still facing the pandemic, allowing for the variation of the Government Stringency Index to in order to understand how effective differing lock downs were, and developing a second model to predict GDP loss in order to find the happy medium between lives saved and GDP losses.

### Discussion
By having a model to predict total number of cases, one becomes able to isolate individual features, under the assumption of independent, so that analysis of government pandemic policies may better prepare the world for future infectious diseases. 

### References
Hale, Webster, Petherick, Phillips, and Kira (2020). Oxford COVID-19 Government Response Tracker - Last updated 28 September, 19:30 (London Time)

“Owid/Covid-19-Data.” GitHub, 21 Apr. 2020, github.com/owid/covid-19-data.

Roser, Max, and Hannah Ritchie. "Coronavirus Disease (COVID-19)." Our World in Data, 4 Mar. 2020, ourworldindata.org/coronavirus.