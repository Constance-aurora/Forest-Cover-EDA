# Forest Cover Type Prediction & Geospatial Analysis

## 1. Project Overview & Business Context
In the context of wildfire management (e.g., recent Southern California Wildfires and Australian Black Summer Bushfires), accurate environmental models are crucial for risk assessment. Modern forestry relies heavily on drones for inspection. 

**Objective:** This project aims to predict forest cover types (e.g., Spruce/Fir, Lodgepole Pine) based on cartographic variables (Elevation, Slope, Soil Type, etc.) that can be captured by drones. This analysis assists stakeholders—such as land management agencies and environmental scientists—in monitoring forest health and designing fire prevention strategies.

## 2. Dataset
* **Source:** Subset of the Covertype dataset from the UCI Machine Learning Repository (Roosevelt National Forest, Colorado).
* **Size:** 30,860 instances, 54 attributes.
* **Key Features:** * Cartographic: Elevation, Aspect, Slope, Distances to Hydrology/Roadways/Fire Points.
    * Categorical: Wilderness Area (4 types), Soil Type (40 types).
* **Target:** Forest Cover Type (7 classes).

## 3. Tech Stack
* **Language:** Python
* **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn

## 4. Methodology (Data Cleaning & Preprocessing)
The raw data contained missing values, outliers, and irrelevant columns. Key cleaning steps included:

* **Categorical Engineering:** * Condensed 40 binary `Soil_Type` columns and 4 `Wilderness_Area` columns into single categorical features to reduce dimensionality.
    * Handled missing soil types by assigning an "Unknown" category.
* **Advanced Imputation:** * Applied **Wilderness-Area-Based Median Imputation** for numerical features. This approach was chosen because geographical features (like elevation) are geographically clustered; using the median of the specific wilderness area prevents bias from outliers.
* **Outlier Removal:** * Used the IQR method (Interquartile Range) to remove definite outliers (beyond 3*IQR), reducing noise in Slope and Hillshade features.

## 5. Key Insights (EDA)
Exploratory Data Analysis revealed several critical patterns for modeling:

* **Elevation is the Key Predictor:** Boxplot analysis shows clear separation of forest types based on elevation. For instance, *Krummholz* is found at the highest elevations, while *Cottonwood/Willow* dominates the lowest.
* **Class Imbalance:** The dataset is heavily skewed towards *Lodgepole Pine* and *Spruce/Fir*. Future modeling will require stratification or class weighting to prevent overfitting.
* **Geospatial Correlations:** * *Distance to Roadways/Fire Points:* Krummholz and Spruce/Fir tend to be located further from infrastructure compared to other types.
    * *Multicollinearity:* Strong correlations were found between Hillshade variables (9am vs 3pm) and Hydrology distances, suggesting feature selection/reduction (PCA) might be needed for linear models.

## 6. Project Structure
```text
├── forest_cover_eda.ipynb   # Main analysis notebook (Cleaning + EDA)
├── forest_cover.csv         # Raw dataset
└── README.md                # Project documentation
