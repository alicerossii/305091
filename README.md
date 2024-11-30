### Project AI on Aliens_Galaxy

## TEAM MEMBERS:
- Alice Rossi 305091;
- Tommaso Giannetti   ;
- Sara Vagheggini  .

## 1.	**Introduction**  
In the future, alien species travel through space and colonize planets all across the galaxy. Each species likes certain types of planets, and their success depends on factors like climate, resources, and how they are socially organized.
The goal of this project is to group colonized planets based on their features to learn which planets attract which species. Using the data from “alien_galaxy.csv”, we will analyze the planets to find patterns and connections. This will help improve colonization plans and support cooperation between different alien species.

## 2.	**Methods**
#### a) Data Understanding
  We have a unique csv file: `alien_galaxy_csv`. The csv has **2240 rows and 34 columns** that describe many aspects of extraterrestrial galaxies, such as scientific, social, and economic characteristics.  

- **Types of Data**:  
  - **Numerical Columns**: Examples include `Ammonia_Concentration`, `Galactic_Trade_Revenue`, and `Alien_Population_Count`.  
  - **Categorical Columns**: Examples include `Alien_Civilization_Level` and `Dominant_Species_Social_Structure`.  
  - **Date Columns**: Examples include `Discovery_Date` and `Colonization_Year`.  
  - **Data Types**: Most columns are numerical, although some, like `Discovery_Date`, are text-based.  

---

#### b)	EDA and Data preparation: 
Explanatory Data Analysis (EDA) is a crucial step in Machine Learning for understanding and preparing the dataset. We used several Python libraries, including:  
- **Pandas**, **Numpy**, **Sklearn**, **Matplotlib.pyplot**, and **Seaborn**.

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

## 3.	**Choice of the 3 methods for the Clustering Problem**
We were required to try 3 models for our Clustering Problem, to find out which one works better on our dataset. We decided to test K-Means; Agglomerative Clustering and DBSCAN. 

### **K-Means**  

The **K-Means algorithm** is a machine learning method used to divide a dataset into clusters based on the following steps:  

1. **Choose the Number of Clusters (`k`)**:  
   Decide how many groups you want to divide the data into.  

2. **Initialize the Cluster Centroids**:  
   The algorithm randomly selects `k` points in the dataset as the initial cluster centroids.  

3. **Assign Each Data Point to the Nearest Centroid**:  
   Each point in the dataset is assigned to the cluster with the closest centroid, based on **Euclidean distance**.  

4. **Recalculate the Centroids**:  
   After assigning the points, the algorithm updates each cluster’s centroid.  
   - The new centroid is the average position of all points in that cluster.  

5. **Repeat Until Convergence**:  
   Steps 3 and 4 are repeated until the centroids stop moving or a predefined number of iterations is reached.  


At the end, the dataset is divided into `k` clusters, each represented by its centroid.  

#### **Optimal Cluster Number Determination**  

- We computed both the **Elbow Method** and the **Silhouette Score** to determine the best number of clusters.  
- After tuning hyperparameters, we found that **3 clusters** was optimal for K-Means, with a silhouette score of **0.56**.  


FOTO DEI K MEANS SIA CON SILHOUTTE SCORE, ELBOW METHOD CHE FOTO 3D

Overall, the plot demonstrates clear separation between clusters, with minimal overlap. This supports the effectiveness of the K-Means algorithm in identifying distinct groups in the dataset.

---
### **Agglomerative Clustering**  

**Agglomerative Clustering** is a machine learning algorithm used to group data in a hierarchical way.  

#### **How It Works**  

1. **Each Point Starts as Its Own Cluster**:  
   Initially, every single data point is considered a separate cluster.  

2. **Find the Two Closest Clusters**:  
   The algorithm measures the distance between all clusters (or points) and identifies the two that are closest to each other.  

3. **Merge the Closest Clusters**:  
   These two clusters are merged to form a new, larger cluster.  

4. **Repeat the Process**:  
   The algorithm keeps finding and merging the closest clusters until there’s only one cluster containing all the data points.  

#### **Results **  

- We performed **hyperparameter tuning**, but the initial results were unsatisfactory:  
  - **2 clusters**  
  - **Average linkage**  
  - Silhouette score: ***0.743***  

- To improve visualization without losing too much on the silhouette score, we explored different settings.  

- Final result:  
  - **3 clusters**  
  - **Ward linkage**  
  - Silhouette score: **0.54**  


*Insert visualizations for Agglomerative Clustering analysis here.*

---

### **DBSCAN**  
**DBSCAN** is a clustering algorithm that groups data points based on their density. Unlike K-Means or Agglomerative Clustering, it doesn’t require you to specify the number of clusters in advance and can identify clusters of different shapes and sizes.
#### **How it Works**  

1. **Find Dense Regions**:  
   - The algorithm identifies regions where many data points are close to each other, forming clusters.  

2. **Key Parameters**:  
   - **Epsilon**: The maximum distance two points can be from each other to be considered neighbors.  
   - **Min Samples**: The minimum number of points needed in a neighborhood to form a dense region.  

3. **Classify Points**:  
   - **Core Points**: Points with at least `Min Samples` neighbors within a distance `Epsilon`.  
   - **Border Points**: Points close to a core point but without enough neighbors to be core points themselves.  
   - **Noise Points**: Points that don’t belong to any cluster.  

4. **Expand Clusters**:  
   - Starting from a core point, DBSCAN groups together all reachable points to form a cluster.  

5. **Repeat**:  
   - The process continues until all points are processed.
  
#### **Results **  

Despite hyperparameter tuning, we were not satisfied with the results obtained using the “best parameters.”  

- We attempted the algorithm with **3 clusters**, assuming this was optimal (as suggested by K-Means results).  
- However, the silhouette score remained very low (**0.19**), and the visualization did not improve either.  

**Reasons for Poor Performance on Our Dataset**:  

1. **High Number of Outliers**:  
   - DBSCAN identifies sparse areas as noise or outliers.  
   - Since our dataset contains many outliers, the method failed to form meaningful clusters.  

2. **Small Dataset Size**:  
   - DBSCAN struggles with smaller datasets where density differences are less pronounced.
  

*Insert  visualizations for DBSCAN analysis here.*

---
  
### Clustering as an Unsupervised Learning Method  

Since clustering is an **unsupervised learning method**, it does not rely on labeled data. Therefore:  

- Metrics like **accuracy**, **precision**, **recall**, or **F1 score** are unsuitable for evaluation.  
- Instead, the focus is on:  
  - **Silhouette Score**  
  - **Cluster Density**  
  - **Cluster Separation**  

Similarly, **train, validation, and test sets** are not used in clustering since the entire dataset is analyzed to uncover inherent patterns rather than to predict outcomes.  

---
  
### Best model? K-Means. Why?

We chose **K-Means Clustering** for the following reasons:  

- **Clear Patterns**:  
  - The clusters generated by K-Means effectively capture key differences, such as energy and resource usage, while being more evenly distributed.  

- **Balanced Clusters**:  
  - K-Means produces clusters that are easier to interpret without losing important trends, making it suitable for general analysis.  

- **Flexibility**:  
  - Unlike Agglomerative Clustering, K-Means avoids rigid hierarchical relationships.  
  - Handles data more consistently than DBSCAN, which tends to focus on extremes and outliers.  


### Final Considerations  

While K-Means and Agglomerative Clustering produced similar cluster densities, K-Means achieved a **higher silhouette score**, indicating:  
- Better-defined clusters.  
- More cohesive groupings.  

This balance of simplicity, interpretability, and performance made **K-Means the best choice** for our analysis.

---

## Analysis of the Clusters of K-Means  

*Insert histograms and visualizations for K-Means analysis here.*

By analyzing each cluster of the K-Means, we identified the factors that attract aliens to each planet:  

### Cluster Descriptions  

- **Cluster 0 (Blue)**:  
  - Generally, Cluster 0 has lower values across most features, even with some negative values.  
  - Represents entities with minimal activity or influence in areas such as:  
    - **Military Engagements**  
    - **Energy Population Impact**  
    - **Terraforming Initiatives**  
  - This cluster corresponds to a group that is resource-limited, less expansive, and has minimal intergalactic presence.  

- **Cluster 1 (Orange)**:  
  - Cluster 1 shows moderate values across the features, maintaining a relatively balanced profile.  
  - Notable peaks in:  
    - **Interplanetary Communications**  
    - **Resource Allocation Credits**  
  - Suggests entities focusing on communication and financial planning.  
  - Represents a middle-ground group with:  
    - Average resource usage and influence.  
    - Potential prioritization of strategic resource management.  

- **Cluster 2 (Green)**:  
  - Dominates most variables, with significantly higher values in key features such as:  
    - **Mineral Extraction Tons**  
    - **Energy Population Impact**  
    - **Terraforming Initiatives**  
  - These entities appear to be:  
    - Resource-intensive.  
    - Highly active in planetary modification.  
    - Focused on expansion.  
  - High values in:  
    - **Military Engagements**  
    - **Species Expansion Response**  
    - Indicate a group prioritizing territorial growth and defense.  

---

### **Conclusion**  

- **Cluster 0**: Represents a less active, resource-limited group with minimal influence.  
- **Cluster 1**: More balanced, prioritizing communication and resource planning.  
- **Cluster 2**: The dominant cluster, with high resource utilization, energy use, and expansion efforts.  

This visualization confirms that the **K-Means algorithm** successfully grouped entities with distinct behavioral patterns, making it easier to interpret the roles and activities of each cluster.  
