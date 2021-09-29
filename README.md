# Introduction
the assignment was to classify astronomical object from the perspective of data analyst standpoint.
# Downloading the Data

(https://github.com/thespacedoctor/astro-analytathon/tree/master/data)

# Dataset
 - 645 objects, each with a lightcurve
 - Lightcurve contains measurements over about 5 years
 - Most points will be at zero flux (within the errors)
 - Different shapes, timescales, peak values : star exploded sometime over last 5 years
 - Supernovae can be more luminous than 10 11 Suns, or brighter than a whole galaxy
 - They can last somewhere between 1 6 months
 - ATLAS is effectively a large camera : observes 4 exposures (each 30 seconds) at each position each night
 
 # Data Pre-Processing
 As all the information about the objects are provided in the format of txt file. The first step is
to build a function that simulate the data as one dataframe and export it as one csv file. The
simulation steps are as follows :
1. Set the directory of the text files
2. Import the os package and return the list of file names using the os method
3. Define a function to separate the white space between the column and return it as a list
4. Create a loop function to iterate all the txt files and append each object into a dataframe <br/>

A glimpse of the dataset is as follows:

![pre-processing result](https://i.ibb.co/p4n9vDH/image5.png)

# Data Cleaning and Exploratory Data Analysis

Firstly, the data is investigated via Exploratory Data Analysis (EDA). A few variable plots are also visualised in
order to further analyse the characteristic of the data.

![pre-processing result](https://i.ibb.co/sbQtzcS/image6.png)

Based from the EDA and the boxplot:
 1. Significant gap value of std and min,max and mean are observed.
 3. Presence of outliers for all the variable uJy, duJy and Chi/N
 4. Negative values are seen in “uJy” and “duJy”
 5. Extreme single high points / low points
Thus, further data cleaning is required which includes: <br>

**1. Removing of outliers and extreme values**
    
- Dropping missing values
- Removing data for Chi/n more than 20,000 & negative values<br>
- Focusing on orange filter only<br>
- Dropping observations for duJy > 5,000<br>
- Includes observations for uJy more than -5,000 and less than 20,000 only<br>

**2. Solution of discontinuity**
    
- To address this issue on discontinuity where the base value of uJy is negative<br>
- The value of median for all negative observation is calculate.<br>
- All the value of uJy is increased by the median value.<br>
- This will then lift the baseline for the uJy values<br>
- All the negative value after the shifting will be remove from the dataset</p>

![cleaning reuslt](https://i.ibb.co/F5pDvQ9/image7.png)
<p>The result of the data cleaning are visualised above: <br>
    From the boxplot the data tend to have better boundaries for all the observation.<br> Filtering the data for ID 2778, the plot then showcased the baseline value at uJy = 0. It also showed a period where the value of uJy increased implying supernovae is occuring, followed by the decaying value right after. </p>
    
<p>The next step is to build the feature variables based on the filtered dataset. <br>

#  Feature Constructing

<p> Creating features are important as most data analytics algorythm will produce results based on the data provided.<br>
    In this project, the two main features that will be used are:</p>
    
1. Flux value during supernovae(uJy)<br>
2. Duration of the supernova
    
<p>However due to the limitation of astronomical knowledge the following key assumptions used are: </p>

1. Defining the active values/supernova when the flux value is more than 5 times the value of the error<br>
2. The duration is the length of observation when (1) is achieved.<br>
3. The mean value flux is taken as representation for each object.<br>

The glimpse of the features are as follows:

![data head](https://i.ibb.co/27nWX5m/image8.png)
# Results and Methodology

The analysis will be using 2 methods:<br>

**1. K-Means Clustering** <br>

K means tend to work well on large sample size and medium number of clusters. It also comes with an elbow method is an algorithm that runs a simulation to determine the best number of cluster

**2. K-Nearest Neighbour (Ward Method)**<br>

This method involves an agglomerative clustering algorithm. it works by repeatedly merging the closest two clusters but use different definitions of the distance between clusters. It is most appropriate for quantitative variables

## K-Means Clustering
The elbow algorithm results are as follows: 

![elbow](https://i.ibb.co/ZdcKnf6/image9.png)
based from the result, it is recommended that the number of cluster is equals to 5

![result1](https://i.ibb.co/m5Hskbk/image10.png)

The result of the analysis is presented in the chart above. Based from the plot, there are 1 major cluster 1 which has 418 objects (orange), 3 minor clusters (red, blue and green) which has 115, 85 and 25 objects respectively, and 1 outlier cluster (purple) which only has 2 objects. The uJx value seems to heavily differentiating the clusters. However, the duration tend to separate the cluster Blue and Orange as well as these 2 clusters are having approximately equal value of flux (uJy). <br>

There is also an outlier cluster which only consist of 2 objects, which are uuid 2916 and 2938. These 2 objects have extremely high value of flux which separate them from most of other objects.The duration of these 2 objects are 37 and 72 respectively. Further analysis should be done to study whether the flux value are accurate. There is a possibility that these value of flux are magnified due to the distant of the object are nearer compared to other objects.<br>

next, the data is analysed using the Ward Method. 

![result2](https://i.ibb.co/c6drvJN/image11.png)

Similar results are observed even when classifying using the Ward Method. The clusters are still consist of 1 Major cluster, 3 minor and 1 outlier clusters. The object distribution for each clusters are similar to the result of K means Algorithm. Hence we can conclude that the classifier method would return the same results if the same data is used as an input.

# Conclusion

The objects are heavily clustered based on the Flux value (uJy), the duration variable also contribute towards classifying the object however it was less significant. Further analysis to be done on object uuid 2916 and 2938 as the flux value are extremely too high. This value could be biased as the distance of the objects were not considered / included in the calculation during constructing the features. 

In the nutshell, it is recommended to analyse the result in the perspective of astronomical standpoint in order to evaluate the results accuracy. In the case where the classification is inaccurate, the features could be improve by using the provided formula. This could improve the features representation of the flux value and also the duration of the supernova. 






