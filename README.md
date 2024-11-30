### Project AI on Aliens_Galaxy

## TEAM MEMBERS:
- Alice Rossi 305091;
- Tommaso Giannetti   ;
-  Sara Vagheggini  .

## 1.	**Introduction**  
In the future, alien species travel through space and colonize planets all across the galaxy. Each species likes certain types of planets, and their success depends on factors like climate, resources, and how they are socially organized.
The goal of this project is to group colonized planets based on their features to learn which planets attract which species. Using the data from “alien_galaxy.csv”, we will analyze the planets to find patterns and connections. This will help improve colonization plans and support cooperation between different alien species.

## 2.	**Methods**
#### a) Data Understanding
•	We have a unique csv file:  “alien_galaxy_csv” . The csv has 2240 rows and 34 columns which describe many aspects of extraterrestrial galaxies, such as scientific, social, and economic characteristics. It has numerical columns such as `Ammonia_Concentration`, `Galactic_Trade_Revenue`, and `Alien_Population_Count`, as well as categorical columns like `Alien_Civilization_Level` and `Dominant_Species_Social_Structure`. Dates and years, such `Discovery_Date` and `Colonization_Year`, are also included. Most data types are numerical, although some columns, such as `Discovery_Date`, are text-based. 


#### b)	EDA and Data preparation: 
We wanted a clean and comprehensible dataset to have a better insight of the data. In this part of our project, we handled missing values and outliers.

•	_Missing values_: we firstly visualize them with a function, and then we plotted the “missing data matrix” that showed us a lot of blank spaces we needed to fill.
  We applied One hot encoding to convert the categorical column Dominant_Species_Social_Structure into numerical one, while for `Alien_Civilization_Level` we did a mapping by hand
  (we didn’t map by hand also the `Dominant_Species_Social_Structure` as we did for `Alien_Civilization_Level` because we didn’t need an ordered mapping).
  
  To handle missing values, we first tried different imputation approaches based on the kind of variable: mean, median, and mode. Each approach was chosen to address the distinct properties of the data classes.
  - Mean Imputation: Used for continuous variables with a normal distribution to replace missing values with the average. However, it distorted data for skewed distributions or outliers.
  - Median Imputation: Suitable for skewed or outlier-prone continuous variables, providing robust estimates but failing to capture variable correlations.
  - Mode Imputation: Applied to categorical and discrete variables by replacing missing values with the most frequent value, but it created artificial clusters.
    
  We lastly decided to fill the missing values with KNN-imputer; in fact this method provides a more dynamic solution by imputing missing values using patterns and linkages between similar data points. It kept the natural variance while better accounting for feature correlations. To maximize the performance of the KNN Imputer, we scaled all variables before to imputation.

•	_Scaling_: We scaled our data before imputing values with the KNN, and we chose the StandardScaler because we chose to remove outliers, so we didn’t need a resilient model to the outliers such as the Robust Scaler, and neither the MinMaxScaler.

•	_Outliers_: we detected and tracked outliers for numerical columns in the DataFrame using the IQR method (the range between the first and third quartiles), and then plotted the boxplot to visualize them. Then we removed the previously identified rows and plotted the new figure, that had less outliers. The remaining outliers were less then the 4 percent, so we decided that keeping these outliers was acceptable.

Our first approaches were to substitute the outliers with the mean, but it was unsuitable as it still distorted the data distribution; and to handle outliers with a logarithmic transformation, which reduced their impact but made interpretation harder and didn’t fully resolve skewness. Ultimately, we opted for the IQR method with removal for cleaner results. But to ensure only the most extreme cases were flagged, we set a high threshold of 4 times the IQR. A lower threshold risked removing too many data points, potentially eliminating valuable information.

•	**Correlation matrix**: we firstly plotted the whole correlation matrix, with all the features. This helped us a lot in seeing the ones we could drop, as they were not correlated with anyone. After this passage, we plotted the correlation matrix again but only with the variables we considered useful. 

•	**Feature engineering**: thanks to the correlation matrix and the pairplot of all of our variables, we noticed that we could combine some of them because of how highly correlated they were. After this, we had all of our final features, and we made a pairplot of all of them together. 

•	_PCA_: This step has been done in the last part of the project; We had to define the PCA to reduce the number of features in a dataset while retaining as much information (variance) as possible. After this, in the last part of the project, we will define it with 2 components for a 2-dimensions figure, and with 3 components for a 3-dimensions figure. We will store the result of the PCA transformation in aliens_pca.





