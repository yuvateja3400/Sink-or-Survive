
---

# Titanic Data Analysis: Pre-Processing and Data Visualisation

## Overview
This project involves a comprehensive analysis of the Titanic dataset, focusing on pre-processing, cleaning, and visualising the data to uncover patterns related to passenger survival. The goal is to understand various factors influencing survival rates aboard the Titanic.

## Dependencies
The following Python libraries are imported for data manipulation and visualisation:
*   `pandas` as `pd`
*   `numpy` as `np`
*   `matplotlib.pyplot` as `plt`
*   `seaborn` as `sns`

The Titanic dataset itself is loaded directly from Seaborn.

## Dataset Information
The dataset initially contains **891 entries** and **15 columns**. Key columns include:
*   `survived` (0 = No, 1 = Yes)
*   `pclass` (Passenger Class)
*   `sex` (Gender)
*   `age`
*   `sibsp` (Number of siblings/spouses aboard)
*   `parch` (Number of parents/children aboard)
*   `fare`
*   `embarked` (Port of Embarkation)
*   `class` (Passenger Class as category)
*   `who` (man, woman, child)
*   `adult_male` (Boolean)
*   `deck` (Deck level)
*   `embark_town` (Town of Embarkation)
*   `alive` (Yes/No)
*   `alone` (Boolean)

The dataset contains both categorical and numerical values.

## Data Pre-processing Steps

### 1. Missing Value Handling
*   **Initial Missing Values**:
    *   `age`: 177 missing values.
    *   `embarked`: 2 missing values.
    *   `deck`: 688 missing values (very high).
    *   `embark_town`: 2 missing values.
*   **Actions Taken**:
    *   The `deck` column was **dropped** due to a large number of missing values.
    *   Missing values in the `age` column were **imputed with the median age**.
    *   Missing values in the `embarked` column were **imputed with the mode** (most frequent value).
    *   Rows with missing values in `embark_town` were **dropped**.
*   **Result**: After handling, there are no missing values remaining in the dataset. The dataset now has 889 entries.

### 2. Data Type Conversion
Several columns were converted to a `category` data type to ensure proper analysis of categorical data:
*   `sex`
*   `embarked`
*   `who`
*   `class` (already category, but confirmed)

### 3. Duplicate Removal
*   **Initial Duplicates**: 116 duplicate rows were identified in the dataset.
*   **Action Taken**: Duplicate rows were **removed**.
*   **Result**: The dataset now has 773 entries, with 0 duplicates remaining.

### 4. Outlier Detection and Treatment
Outliers are data points significantly deviating from others, potentially due to errors or genuine variability.
*   **Outliers Detected (before treatment)**:
    *   `fare`: 100 outliers.
    *   `age`: 26 outliers.
*   **Method Used**: The **Interquartile Range (IQR) method** was used to identify outliers.
    *   `lower_bound = Q1 - 1.5 * IQR`
    *   `upper_bound = Q3 + 1.5 * IQR`
*   **Handling Strategy**: **Capping** was chosen to treat outliers. This involves adjusting values above the upper bound to the upper bound, and values below the lower bound to the lower bound.
    *   **Why Capping?**: It limits the influence of extreme values, stabilises the dataset for analysis, is straightforward to implement, and retains all data points, preserving dataset size and integrity.
*   **Result**: Box plots after treatment show that outliers in 'fare' and 'age' have been successfully capped within the defined bounds.

## Data Analysis and Visualisation

Data analysis by visualisation is a critical step for understanding patterns, anomalies, and relationships within the dataset.

### 1. Uni-Variate Analysis
This focuses on examining one variable at a time to understand its distribution, central tendency, and variability.
*   **Age Distribution**: A histogram with KDE shows the distribution of ages, with a mean age of approximately **29.53 years**.
*   **Fare Distribution**: A histogram with KDE shows the distribution of fares, with a mean fare of approximately **34.76 units**.
*   **Gender Distribution**: A count plot shows the distribution of passengers by sex, indicating more male passengers than female.
*   **Distribution of Passenger Type (`who`)**: A count plot shows categories of 'child', 'man', and 'woman', indicating a significant number of 'men' aboard.

### 2. Bi-Variate Analysis
This involves the statistical analysis of two variables to determine their empirical relationship.
*   **Fare vs Age (by Survival Status)**: A scatter plot indicates:
    *   **Higher fares are associated with higher survival likelihood** (orange dots).
    *   Both survivors and non-survivors are distributed across all age groups.
    *   A notable cluster of **young children (ages 0-10) survived**.
    *   Non-survivors (blue) are densely clustered at lower fare ranges, suggesting **lower fare passengers had a lower survival rate**.
*   **Age Distribution vs Survival Status**: A box plot shows:
    *   Both groups have **similar median ages (around 28-30 years)**.
    *   **More older individuals did not survive**, appearing as outliers in the non-survivor group.
    *   The lower age range for survivors indicates that **more young children survived**.
*   **Fare Distribution vs Survival Status**: A box plot shows:
    *   **Higher fares are associated with higher survival rates**, with survivors having a broader interquartile range (IQR).
    *   **The majority of non-survivors paid lower fares**, aligning with the scatter plot observations.
    *   Outliers for survivors reach much higher fare values (up to 500 units), supporting that **some high-fare passengers survived**.

### 3. Multi-Variate Analysis
This involves statistical techniques to analyse data with more than two variables, aiming to predict survival based on factors like age, fare, passenger class, and gender.
*   **Correlation Matrix (Heat Map)**: This visualisation is used for numerical values to identify highly correlated variables.
    *   **Positive correlation (0.25) between survival and fare**: Higher fares link to higher survival.
    *   **Negative correlation (-0.33) between survival and passenger class**: Higher-class passengers were more likely to survive.
    *   **Strong negative correlation (-0.55) between fare and passenger class**: Higher classes paid higher fares.
    *   **Moderate positive correlation (0.38) between `sibsp` and `parch`**: Indicates families traveling together.
*   **Pair Plots**: Visualise relationships between multiple variables, with hue based on survival status, showing distributions and scatter plots for all pairs of numerical variables.

## Feature Engineering
New features were created from existing ones to capture additional information:
1.  **`Family_Size`**: Calculated as `sibsp + parch + 1` (accounting for the passenger themselves).
2.  **`Is_Alone`**: A binary feature (`1` if `Family_Size = 1`, `0` otherwise). This helps understand survival rates for solo travellers.
3.  **`Fare_Per_Person`**: Calculated as `fare / Family_Size`. This provides insight into the financial burden per individual.

## Visualising the New Features
Visualisations help in understanding the distribution and impact of the newly created features:
*   **Family Size Distribution**: A count plot shows how many passengers travelled alone versus with family members.
*   **Is Alone Distribution**: A count plot provides insight into the proportion of passengers travelling alone, which could correlate with survival rates.
*   **Fare Per Person Distribution**: A histogram with KDE visualises the distribution of fare expenses per individual, offering insights into the economic backgrounds.

---
