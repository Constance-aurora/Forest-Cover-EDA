# Forest Cover Type: Geospatial Data Cleaning & EDA Pipeline

## 1. Project Overview & Business Context
In the domain of wildfire management and ecological monitoring, the quality of data determines the accuracy of predictive models. Real-world sensor data is often messy, containing noise, outliers, and missing values.

**Objective:** This project focuses on the **critical "Stage 1" of the Data Science lifecycle**: transforming a raw, noisy dataset of cartographic variables (Elevation, Slope, Soil Type) into a high-quality, analysis-ready format.

Instead of jumping straight into modeling, this project performs rigorous **Data Quality Assurance** and **Exploratory Data Analysis (EDA)** to determine which geospatial features are most viable for predicting forest cover types in the Roosevelt National Forest.

**Key Achievements:**
* **Domain-Driven Cleaning:** Handled complex missing data patterns using domain-specific imputation strategies (Wilderness-Area based).
* **Outlier Detection:** Identified and removed geospatial outliers that could bias future models.
* **Feature Engineering:** Reduced dimensionality by condensing 44 binary columns into categorical variables.

## 2. Dataset
* **Dataset Used:** A modified, "messy" version of the Forest Cover Type dataset (included in this repo as `forest_cover.csv`).
* **Original Source:** [Forest Cover Type Prediction (Kaggle/UCI)](https://www.kaggle.com/c/forest-cover-type-prediction/data)
* **⚠️ Important Note:** The code in this repository is specifically engineered to handle the **noise and missing values introduced in the modified dataset**. It is **NOT** directly compatible with the clean version found on Kaggle without adjustments.
* **Target:** Forest Cover Type (7 classes).
* **Key Features:** Elevation, Aspect, Slope, Distances to Hydrology/Roadways, Hillshade, Soil Type.

## 3. Tech Stack
* **Language:** Python
* **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn

## 4. Methodology (Data Cleaning Pipeline)
The raw data presented significant quality issues. The cleaning pipeline implemented in `forest_cover_eda.ipynb` includes:

1.  **Categorical Data Processing:**
    * Merged 40 one-hot encoded `Soil_Type` columns into a single categorical feature.
    * Merged 4 `Wilderness_Area` columns into a single feature.
    * Handled zero-vectors in soil types by assigning an "Unknown" category.

2.  **Advanced Imputation Strategy:**
    * Instead of a simple mean/median fill, **Wilderness-Area-Based Median Imputation** was used.
    * *Rationale:* Geospatial features like Elevation vary significantly between wilderness areas. Using the global median would introduce bias; grouping by area preserves the local environmental characteristics.

3.  **Outlier Removal:**
    * Applied the IQR (Interquartile Range) method (threshold: 3 * IQR) to remove extreme outliers in `Slope` and `Hillshade` which were identified as measurement errors.

## 5. Key Analytical Insights (EDA)
Through comprehensive EDA, the following patterns were identified to guide future modeling:

### 1. Elevation is the Key Predictor
Boxplot analysis reveals distinct elevation bands for different forest types. For instance, *Krummholz* is consistently found at the highest elevations, while *Cottonwood/Willow* dominates the lowest elevations. This clear separation suggests that Elevation will likely be the most important feature in any decision-tree based model.

### 2. Feature Correlations and Multicollinearity
The correlation matrix indicates strong relationships between Hillshade variables (specifically 9am vs 3pm) and Hydrology distances. This suggests that future linear models would require dimensionality reduction (like PCA) or rigorous feature selection to avoid multicollinearity issues.

### 3. Class Imbalance
The target variable analysis shows a heavy skew towards *Lodgepole Pine* and *Spruce/Fir*. The other five forest types are underrepresented. Any future predictive model must employ techniques like **SMOTE** (Synthetic Minority Over-sampling Technique) or **Class Weighting** to prevent the model from ignoring the minority forest types.

## 6. Future Work / Next Steps
With the data now cleaned and patterns identified, the next stages of this project would involve:
1.  **Feature Selection:** Removing highly correlated features identified in the Heatmap.
2.  **Model Selection:** Training a **Random Forest** or **XGBoost** classifier, as these models handle non-linear geospatial relationships better than linear regression.
3.  **Pipeline Automation:** Building a scikit-learn `Pipeline` to automate the cleaning steps defined in this notebook for production use.

## 7. How to Run
1.  Clone the repository.
2.  Ensure `forest_cover.csv` is in the same directory.
3.  Run the Jupyter Notebook:
    ```bash
    jupyter notebook forest_cover_eda.ipynb
    ```
