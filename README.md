# Cooking Up Data: Understanding Recipe Ratings and Preferences

Author: Rayyan Khalid

## Overview

The goal of this project is to analyze factors that impact the average ratings of recipes and predict future ratings based on recipe attributes such as the number of ingredients, preparation steps, and calorie content. Using a dataset of over 80,000 recipes and user interactions, we clean and explore the data, test hypotheses, build predictive models, and ensure fairness in predictions across recipe categories. This project not only highlights trends in recipe preferences but also addresses the challenges of creating equitable models for diverse data.

## Introduction

This project investigates the key factors that contribute to higher average ratings of recipes on Food.com, a popular recipe-sharing platform. The dataset used in this analysis comprises of two parts: a 'recipes' dataset and an 'interactions' dataset. The recipes dataset contains detailed information about over 83,000 recipes, including preparation time, nutritional details, and ingredients. The interactions dataset provides over 730,000 user reviews and ratings, offering insights into user preferences and behavior. By combining these datasets, we aim to explore trends in recipe ratings and predict future ratings based on recipe attributes.

### Research Question
**What types of recipes tend to have higher average ratings?**  
This question helps uncover the factors that make a recipe popular among users. Insights derived from this analysis can aid chefs, food bloggers, and recipe developers in creating recipes that resonate with their audience.

### Dataset Description
The datasets include the following columns:

#### Recipes Dataset
| Column Name      | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `name`           | Name of the recipe, providing a unique identifier for each dish.           |
| `id`             | Unique Recipe ID.                                                          |
| `minutes`        | Total preparation time for the recipe (in minutes).                        |
| `contributor_id` | ID of the user who submitted the recipe.                                    |
| `submitted`      | Date when the recipe was submitted.                                         |
| `tags`           | Categories associated with the recipe (e.g., cuisine or meal type).         |
| `nutrition`      | Nutritional information including calories, fat, protein, and carbohydrates.|
| `n_steps`        | Number of steps in the recipe instructions.                                 |
| `steps`          | Detailed recipe instructions.                                              |
| `description`    | User-provided description of the recipe.                                   |
| `ingredients`    | List of ingredients used in the recipe.                                     |
| `n_ingredients`  | Total number of ingredients used.                                           |
| `average_rating` | The average rating of the recipe, calculated from user interactions.        |

#### Interactions Dataset
| Column Name  | Description                                                       |
|--------------|-------------------------------------------------------------------|
| `user_id`    | ID of the user who provided the rating or review.                 |
| `recipe_id`  | ID of the recipe being reviewed.                                  |
| `date`       | Date of the interaction (review or rating).                       |
| `rating`     | Rating given to the recipe by the user.                           |
| `review`     | Textual review provided by the user (optional).                   |

### Importance of the Study
Understanding the elements that influence recipe ratings has both practical and academic significance. It helps recipe developers identify traits that users find appealing, such as simplicity or nutritional content, and empowers food enthusiasts with insights to discover recipes they might enjoy. Moreover, this project showcases robust data analysis and predictive modeling techniques that can be applied to other recommendation systems.

## Data Cleaning and Exploratory Data Analysis (EDA)

### Data Cleaning
To prepare the dataset for analysis, the following cleaning steps were performed:

1. **Replaced Invalid Ratings with Missing Values:**
   In the interactions dataset, ratings of 0 were replaced with `NaN` because a rating of 0 is likely to represent missing or invalid data rather than a true user rating.

2. **Merged the Datasets:**
   The `recipes` and `interactions` datasets were merged using a left join on the `id` and `recipe_id` columns. This ensured that every recipe in the `recipes` dataset was retained, even if it did not have user interactions.

3. **Calculated Average Ratings:**
   Computed the average rating for each recipe using the merged dataset and added this as a new column, `average_rating`, to the `recipes` dataset.

4. **Extracted Nutritional Information:**
   Split the `nutrition` column (stored as a string) into separate columns for:
   - calories
   - total_fat
   - sugar
   - sodium
   - protein
   - saturated_fat
   - carbohydrates

   This allowed for easier analysis of nutritional factors and their relationship with ratings.

5. **Handled Missing Values:**
   Checked for missing values in key columns (`average_rating`, `nutrition`, `minutes`) and confirmed they were expected based on the data-generating process. These missing values will be handled as needed during analysis.

**Effect of Data Cleaning on Analysis:**
- Replacing invalid ratings and computing average ratings ensures accurate insights into recipe popularity.
- Splitting the `nutrition` column facilitates the analysis of specific nutritional factors and their relationship with ratings.
- Merging the datasets provides a comprehensive view of recipes and user preferences, laying the foundation for further exploration.

---

### Univariate Analysis

#### Plot 1: Distribution of Average Ratings
The histogram shows the distribution of `average_rating` for recipes.
- **Key Observations:**
  - Most recipes have high average ratings (4.0 to 5.0).
  - There is a noticeable skew towards higher ratings, suggesting that users generally rate recipes favorably.

<iframe
  src="assets/Average_Ratings_Distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Plot 2: Distribution of Calories in Recipes
The histogram depicts the calorie distribution of recipes.
- **Key Observations:**
  - The majority of recipes have calorie counts clustered around the lower end of the spectrum.
  - There are some recipes with significantly higher calorie values, creating a long tail in the distribution.

<iframe
  src="assets/Calories_Distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Bivariate Analysis

#### Scatter Plot: Calories vs. Average Rating
This scatter plot shows the relationship between the calorie count of recipes and their average ratings.
- While there is no clear trend, most recipes with high ratings are distributed across a range of calorie values, suggesting that calorie count alone may not significantly influence ratings.

<iframe
  src="assets/Scatter_plot_Cal_vs_AvgRatings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### Box Plot: Average Rating by Number of Ingredients
The box plot displays how the number of ingredients in a recipe affects its average rating.
- Recipes with fewer ingredients tend to have more consistent ratings.
- Recipes with a higher number of ingredients show greater variability in ratings, possibly reflecting the complexity and varying success of these recipes.

<iframe
  src="assets/Boxplot_AvgRating_by_no_of_Ingred.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Interesting Aggregates

#### Grouped Table: Recipes by Number of Ingredients
This table shows the number of recipes and the average rating (mean) grouped by the number of ingredients. It highlights how the number of ingredients affects the distribution and popularity of recipes. Simpler recipes with fewer ingredients tend to have more consistent ratings, reflecting their widespread appeal.

|   Average Rating (Mean) |   Number of Recipes |
|------------------------:|--------------------:|
|                 4.86154 |                  13 |
|                 4.69258 |                 723 |
|                 4.66203 |                2280 |
|                 4.63394 |                4348 |
|                 4.64743 |                6355 |

#### Pivot Table: Calories and Ratings by Number of Steps
This pivot table examines the average calories and average ratings of recipes based on the number of steps required. It provides insight into the complexity of recipes and their nutritional content, showing how the number of steps correlates with user preferences and calorie counts.

|   average_rating |   calories |
|-----------------:|-----------:|
|          4.64813 |    285.133 |
|          4.66612 |    293.095 |
|          4.65546 |    307.189 |
|          4.64004 |    344.003 |
|          4.61038 |    358.074 |


## Assessment of Missingness

### NMAR Analysis: Understanding Missingness in `average_rating`
The `average_rating` column likely exhibits characteristics of NMAR (Not Missing At Random). This means that the missingness in this column is tied to unobservable factors, specifically the subjective decisions of users, rather than any explicit feature in the dataset.

#### Why is `average_rating` NMAR?
1. **Reasoning About User Behavior**: Users might choose not to leave a rating if they:
   - Did not prepare the recipe as instructed.
   - Have no strong opinion about the recipe (neither liked nor disliked it).
2. **Explaining the Missingness**: These choices are linked to user-specific preferences or habits, which are not captured in the dataset. For instance, some users may consistently avoid rating recipes regardless of their experience.

#### How Could We Model the Missingness as MAR?
To shift the missingness from NMAR to MAR (Missing At Random), additional data about user behavior would be required, such as:
- **User Interaction Patterns**: How frequently a user rates recipes.
- **Contextual Information**: Whether a user viewed, saved, or printed the recipe.
- **Recipe Engagement**: Whether the recipe was bookmarked or marked as a favorite.

These insights could provide observable factors to explain why some users do not leave ratings, allowing us to model the missingness more effectively.

---

### Missingness Dependency: Investigating Relationships
To further explore the missingness in `average_rating`, we conducted permutation tests to determine if the missingness depends on specific columns in the dataset. Below, we summarize our findings for `sodium` (independent column) and `minutes` (dependent column).

#### Sodium and Missingness
- **Null Hypothesis**: The missingness of `average_rating` does not depend on the sodium content of the recipe.
- **Alternate Hypothesis**: The missingness of `average_rating` depends on the sodium content of the recipe.
- **Test Statistic**: The absolute difference in the mean sodium content between recipes with missing and non-missing `average_rating` values.
- **Results**:
  - Observed Test Statistic: **0.35**
  - P-value: **0.90**

<iframe
  src="assets/Permutation_Test_missingness_sodium.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Conclusion**: Since the p-value is much greater than 0.05, we fail to reject the null hypothesis. The missingness of `average_rating` does not depend on the sodium content.  
**Insert Sodium Permutation Test Histogram Here**

#### Minutes and Missingness
- **Null Hypothesis**: The missingness of `average_rating` does not depend on the preparation time (`minutes`) of the recipe.
- **Alternate Hypothesis**: The missingness of `average_rating` depends on the preparation time (`minutes`) of the recipe.
- **Test Statistic**: The absolute difference in the mean preparation time (`minutes`) between recipes with missing and non-missing `average_rating` values.
- **Results**:
  - Observed Test Statistic: **117.34**
  - P-value: **0.04**
 
<iframe
  src="assets/Permutation_Test_missingness_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Conclusion**: Since the p-value is less than 0.05, we reject the null hypothesis. The missingness of `average_rating` depends on the preparation time (`minutes`) of the recipe.  
**Insert Minutes Permutation Test Histogram Here**

---

### Visual Insights
1. **Sodium Dependency**: The histogram of permutation test statistics shows that the observed statistic lies well within the range of simulated values, indicating no dependency.
2. **Minutes Dependency**: The histogram highlights that the observed statistic is significantly larger than most simulated test statistics, confirming dependency.

These findings emphasize the importance of understanding missingness to avoid biases during imputation and subsequent analysis.


## Just testing another step 3

## Assessment of Missingness

### NMAR Analysis: Investigating the Missingness in `average_rating`
The `average_rating` column in our dataset exhibits characteristics of NMAR (Not Missing At Random). The missingness in this column seems to arise from unobservable factors related to user behavior and preferences.

#### Why `average_rating` Might Be NMAR
1. **User Decisions**: Missing ratings might result from users who:
   - Did not prepare the recipe as described.
   - Do not feel strongly about the recipe (neither liked nor disliked it).
2. **Unobservable Factors**: These behaviors are not captured by any of the dataset’s columns, making it impossible to directly model the missingness using the existing features.

#### Making NMAR Missingness MAR
To model this missingness as MAR (Missing At Random), we would need additional data, such as:
- **User-Specific Behavior**: Information on how frequently users rate recipes.
- **Engagement Metrics**: Data indicating whether users viewed, saved, or printed recipes.
- **Recipe-Specific Actions**: Details about whether a recipe was bookmarked or marked as a favorite.

These additional features would allow us to explain why users choose not to leave ratings, potentially transforming the missingness into a MAR structure.

---

### Missingness Dependency: Testing Relationships
To assess whether the missingness of `average_rating` depends on other columns in the dataset, we conducted permutation tests using a mean difference test statistic. Below, we summarize the results.

#### Test Results Summary
The permutation tests were conducted on the following columns: `protein`, `sodium`, `minutes`, `n_ingredients`, and `calories`. The table below summarizes the observed test statistics and p-values:

| Tested Column    | Observed Statistic | P-Value     |
|-------------------|--------------------|-------------|
| `protein`        | 1.29              | 0.202       |
| `sodium`         | 0.35              | 0.880       |
| `minutes`        | 117.34            | 0.044       |
| `n_ingredients`  | 0.25              | 0.002       |
| `calories`       | 87.86             | 0.000       |

#### Interpreting the Results
1. **Independent Column: `sodium`**
   - **Null Hypothesis**: The missingness of `average_rating` does not depend on the sodium content.
   - **Result**: With a p-value of 0.880, we fail to reject the null hypothesis, indicating no significant dependency.  
   **Insert sodium permutation test plot here**

2. **Dependent Column: `minutes`**
   - **Null Hypothesis**: The missingness of `average_rating` does not depend on the preparation time (`minutes`).
   - **Result**: With a p-value of 0.044, we reject the null hypothesis, indicating a significant dependency.  
   **Insert minutes permutation test plot here**

---

### Visual Insights
1. **`sodium` Plot**: The histogram of permutation test statistics shows that the observed statistic lies well within the null distribution, supporting no dependency.  
<iframe
  src="assets/Permutation_Test_missingness_sodium.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

2. **`minutes` Plot**: The histogram shows that the observed statistic is significantly larger than most of the simulated test statistics, supporting dependency.  
<iframe
  src="assets/Permutation_Test_missingness_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Methodology
#### Test Statistic
- The **absolute difference in means** was used as the test statistic to quantify the separation between groups with and without missing values in `average_rating`.

#### Permutation Test Details
1. **Observed Statistic**: Calculated the absolute difference in the mean of the tested column for recipes with missing and non-missing `average_rating`.
2. **Null Hypothesis**: The missingness in `average_rating` is independent of the tested column.
3. **Permutations**: Repeated the process 500 times, shuffling the missingness labels to simulate the null hypothesis.
4. **P-value**: Computed as the proportion of permuted test statistics greater than or equal to the observed statistic.

---

### Conclusion
Through our analysis:
- We determined that the missingness in `average_rating` depends on `minutes` (preparation time) but does not depend on `sodium` (sodium content).
- These insights help us better understand the missingness structure and guide future imputation strategies, ensuring accurate analysis of recipe ratings.


## Hypothesis Testing

### Objective
To determine whether the number of ingredients in a recipe influences its average rating, we conducted a hypothesis test using a permutation approach.

### Hypotheses
- **Null Hypothesis (H₀)**: The average rating of recipes is independent of the number of ingredients (≤10 ingredients vs. >10 ingredients).
- **Alternative Hypothesis (Hₐ)**: The average rating of recipes depends on the number of ingredients.

### Test Statistic
The test statistic is the **absolute difference in the mean average ratings** between two groups:
1. Recipes with ≤10 ingredients.
2. Recipes with >10 ingredients.

### Methodology
1. Calculated the observed test statistic as the absolute difference in means between the two groups.
2. Simulated the null hypothesis by shuffling the group labels (`≤10 ingredients` and `>10 ingredients`) 1,000 times and recalculating the test statistic for each permutation.
3. Computed the p-value as the proportion of permuted test statistics greater than or equal to the observed test statistic.

### Significance Level
- **Significance Level (α)**: 0.05

### Results
- **Observed Statistic**: ~0.0021  
- **P-Value**: ~0.671  

### Visualization
The histogram below illustrates the distribution of test statistics generated under the null hypothesis. The observed statistic is marked by a red vertical line, showing its position relative to the null distribution.

<iframe
  src="assets/permutation_test_step4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Conclusion
Since the p-value (~0.671) exceeds the significance level (0.05), we **fail to reject the null hypothesis**. This result suggests that the average rating of recipes is not significantly influenced by the number of ingredients. 

### Interpretation
The lack of a significant difference implies that other factors, such as preparation time, recipe complexity, or taste preferences, may play a more substantial role in determining average ratings. This finding highlights the multifaceted nature of user preferences and suggests that future analyses should explore additional recipe attributes to better understand what drives user ratings.


## Framing a Prediction Problem

### Prediction Problem Statement
The goal of this analysis is to predict the **average rating** (`average_rating`) of a recipe based on its characteristics, such as:
1. Number of ingredients (`n_ingredients`),
2. Number of preparation steps (`n_steps`), and
3. Caloric content (`calories`).

Accurately predicting `average_rating` can help us understand how recipe complexity and nutritional content influence user satisfaction. This predictive insight can guide recipe creators in designing dishes that align with user preferences.

### Type of Prediction Problem
This is a **regression problem**, as the response variable (`average_rating`) is continuous, ranging from 1 to 5.

### Response Variable
The response variable is **`average_rating`**, which represents the user rating averaged across multiple interactions for a given recipe. This variable was chosen because:
- It captures overall user satisfaction with a recipe, reflecting its appeal and popularity.
- Predicting `average_rating` allows us to explore the relationship between recipe attributes and user feedback.

### Features Used for Prediction
The features selected for prediction include:
1. **`n_ingredients` (Number of Ingredients)**: Reflects the complexity of the recipe. Simpler recipes may appeal to users seeking convenience.
2. **`n_steps` (Number of Preparation Steps)**: Indicates the effort required to complete the recipe. Recipes with fewer steps may attract users who value simplicity.
3. **`calories`**: Represents the nutritional content of the recipe. Extreme caloric values may influence perceptions of healthiness, affecting user ratings.

### Evaluation Metric
To evaluate the performance of the predictive model, we will use the **Mean Absolute Error (MAE)**:
- **Why MAE?**
  - It provides an interpretable measure of the average prediction error in the same units as `average_rating`.
  - MAE is robust to outliers, which is essential given the skewed distribution of `average_rating`.

### Justification for Features and Prediction Timing
The selected features (`n_ingredients`, `n_steps`, and `calories`) are logical predictors of `average_rating`:
- **Complexity**: Captured by `n_ingredients` and `n_steps`, directly relates to user satisfaction and accessibility.
- **Nutritional Content**: Reflected by `calories`, influences health perceptions and overall appeal.

All these features are known at the time of prediction since they are inherent attributes of the recipe.

### Visual Exploration of the Problem
#### 1. **Distribution of `average_rating`**
   - The histogram of `average_rating` reveals a strong skew toward higher ratings, with many recipes clustering at the maximum value of 5.
<iframe
  src="assets/Step5_Distribution_of_Average_Ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### 2. **Scatterplot: `n_ingredients` vs. `average_rating`**
   - Displays no strong linear trend, but recipes with fewer ingredients tend to cluster around higher ratings.
<iframe
  src="assets/Step5_Scatterplot_n_ingredients.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### 3. **Scatterplot: `n_steps` vs. `average_rating`**
   - Suggests that recipes with fewer steps exhibit more consistent ratings, likely due to their simplicity.
<iframe
  src="assets/Step5_Scatterplot_n_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### 4. **Scatterplot: `calories` vs. `average_rating`**
   - Shows that recipes with moderate calorie levels often achieve higher ratings, while extreme calorie counts show no clear trend.
<iframe
  src="assets/Step5_Scatterplot_calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Summary of Observations
- Recipes with fewer ingredients and fewer preparation steps are generally rated more favorably, likely due to their accessibility and ease of preparation.
- Moderate calorie levels appear to correlate with higher user satisfaction, whereas extreme values (either high or low) lack a consistent relationship with ratings.
- The response variable (`average_rating`) is well-suited for regression analysis, as it quantitatively captures user satisfaction.

### Importance of This Prediction Problem
Predicting `average_rating` offers valuable insights for:
1. **Recipe Creators**: Helping them craft recipes that align with user preferences for simplicity, healthiness, or complexity.
2. **Food Enthusiasts**: Enabling them to discover recipes more likely to meet their expectations.
3. **Culinary Data Analysis**: Providing a quantitative basis for understanding how recipe attributes influence user satisfaction.


## Step 6: Baseline Model

### Model Description
The baseline model is a **Random Forest Regressor**, chosen for its robustness and ability to handle both numerical and categorical data effectively. The model is built using a pipeline to streamline preprocessing and ensure reproducibility. By incorporating preprocessing steps within the pipeline, data transformations are applied consistently across the training and test sets, minimizing data leakage.

### Features in the Model
The model incorporates a mix of numerical and categorical features, processed as follows:

#### Numerical Features (6):
1. **`minutes`**: Total time to prepare the recipe.
2. **`n_steps`**: Number of steps required to prepare the recipe.
3. **`n_ingredients`**: Number of ingredients used in the recipe.
4. **`calories`**: Total calorie count of the recipe.
5. **`protein`**: Protein content (as a percentage of the daily value).
6. **`sodium`**: Sodium content (as a percentage of the daily value).

- These numerical features were scaled using **StandardScaler** to ensure equal weighting during model training. Scaling is essential for Random Forest Regressors, as unscaled features with larger ranges could disproportionately influence the splits.

#### Categorical Features (1):
1. **`tags`**: Comma-separated tags describing recipe attributes (e.g., "dessert", "vegetarian").

- The `tags` feature was processed using **OneHotEncoder**, which creates dummy variables for each unique tag. This encoding ensures that categorical information is represented in a format usable by the Random Forest Regressor.

### Evaluation Metric
The **Mean Absolute Error (MAE)** was chosen as the evaluation metric for the baseline model because:
- It provides an interpretable measure of average prediction error in the same units as the target variable (`average_rating`).
- It is robust to outliers, making it suitable for our dataset, where ratings are skewed towards higher values.

### Model Performance
- **MAE**: **0.4891**
  - This result indicates that the model’s predictions deviate, on average, by approximately **0.5 points** from the true recipe ratings. Considering the target variable (`average_rating`) ranges from 1 to 5, this represents a reasonably good baseline performance.

### Discussion

#### Strengths
1. **Inclusion of Diverse Feature Types**:
   - The model leverages both numerical and categorical features, enhancing its predictive power.
2. **Reproducibility**:
   - The pipeline ensures that preprocessing steps (e.g., scaling and encoding) are applied consistently across the dataset.
3. **Handling of Categorical Data**:
   - OneHotEncoder efficiently represents categorical features without introducing biases from ordinal relationships.

#### Weaknesses
1. **Lack of Feature Interactions**:
   - The model does not explicitly account for interactions between features (e.g., how `calories` and `tags` might jointly influence ratings).
2. **Limited Feature Engineering**:
   - Additional insights could be derived from features like recipe descriptions (`description`) or steps (`steps`) through natural language processing (NLP) techniques.
3. **Hyperparameter Tuning**:
   - The model uses default hyperparameters for the Random Forest Regressor, which may not optimize its performance.

#### Areas for Improvement
To enhance the model’s accuracy, the following strategies could be considered:
1. **Hyperparameter Tuning**:
   - Experiment with the number of trees (`n_estimators`), maximum tree depth (`max_depth`), and minimum samples per split (`min_samples_split`) to improve model performance.
2. **Feature Engineering**:
   - Extract additional features, such as the length of recipe descriptions, sentiment analysis of user reviews, or derived metrics (e.g., ingredient diversity).
3. **Accounting for Feature Interactions**:
   - Incorporate methods to model interactions between numerical and categorical features, which may provide richer insights.
  

## Final Model

### Features Added
To enhance the predictive power of the model, the following features were added based on their relevance to the data-generating process:

1. **Interaction Term: `n_steps * n_ingredients`**
   - This feature captures the interaction between the number of steps and the number of ingredients in a recipe. It reflects the complexity of recipes, as recipes with many steps and ingredients are often more challenging to prepare.
   - From the perspective of the data-generating process, more complex recipes may influence user ratings differently, depending on the effort and skill required to execute them. Including this interaction term allows the model to account for such variations.

2. **Log Transformation of `minutes`**
   - A log transformation was applied to the `minutes` column to normalize its distribution, addressing extreme outliers such as recipes with very long cooking times.
   - This transformation reduces skewness, ensuring that the model does not disproportionately weight recipes with unusually long preparation times. By normalizing the feature, the model can better understand how typical cooking times affect user ratings.

These features were selected because they align with the key factors that likely influence recipe ratings. They provide meaningful representations of recipe complexity and preparation time, enhancing the model's ability to capture relationships between features and the target variable.

---

### Modeling Algorithm
The **Random Forest Regressor** was used as the final model due to its strengths:
- Ability to handle both numerical and categorical data.
- Robustness to overfitting due to its ensemble nature.
- Capability to capture non-linear relationships between features.

#### Hyperparameters Tuned:
1. **`n_estimators`**: Number of trees in the forest.
2. **`max_depth`**: Maximum depth of each tree.
3. **`min_samples_split`**: Minimum number of samples required to split an internal node.

#### Method Used for Hyperparameter Tuning:
- **GridSearchCV** was employed with 3-fold cross-validation to explore various combinations of hyperparameters and identify the optimal set.
- **Best Parameters Identified:**
  - `n_estimators`: 200
  - `max_depth`: 10
  - `min_samples_split`: 2

These hyperparameters strike a balance between model complexity and generalizability, ensuring good performance on unseen data.

---

### Performance Comparison
The final model demonstrated a measurable improvement over the baseline model:

- **Baseline Model Performance:**
  - Mean Absolute Error (MAE): **0.4891**
- **Final Model Performance:**
  - Mean Absolute Error (MAE): **0.4640**

This improvement in MAE indicates that the final model predicts recipe ratings more accurately than the baseline. The enhancement can be attributed to:
1. The addition of engineered features that better represent recipe complexity and preparation characteristics.
2. Optimized hyperparameters, improving the model's ability to generalize.

---

### Visualization
The bar chart below compares the performance of the baseline and final models. The reduction in MAE reflects the improvement in prediction accuracy:
<iframe
  src="assets/Step7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Conclusion
The final model significantly improves upon the baseline by incorporating well-thought-out features and optimized hyperparameters. The use of engineered features, such as the interaction term and log transformation, ensures the model captures crucial aspects of the data-generating process. While the performance is strong, further refinements could include testing additional interactions, exploring more advanced models, or incorporating new features from the dataset. This step demonstrates how thoughtful feature engineering and model tuning can elevate the performance of predictive models.


## Fairness Analysis

### Fairness Assessment Overview
Fairness in predictive modeling is essential to ensure that the model performs equitably across different groups. In this project, we analyzed the fairness of the final model by comparing its performance between two distinct groups of recipes based on their preparation times:
1. **Quick Recipes:** Recipes with cooking times ≤ 30 minutes.
2. **Time-Consuming Recipes:** Recipes with cooking times > 30 minutes.

---

### Hypotheses
- **Null Hypothesis (H₀):** The model's performance (measured by Mean Absolute Error, MAE) does not differ significantly between the two groups (Quick Recipes and Time-Consuming Recipes).
- **Alternative Hypothesis (Hₐ):** The model's performance differs significantly between the two groups.

---

### Evaluation Metric
The **Mean Absolute Error (MAE)** was used to measure the model's performance for each group. MAE is appropriate for this analysis because it provides an interpretable measure of average prediction error.

---

### Test Statistic
The **difference in MAE** between the two groups was chosen as the test statistic, reflecting disparities in the model’s performance.

---

### Significance Level
A significance level of **α = 0.05** was used to determine statistical significance.

---

### Results
- **Observed MAE Difference:** 0.0407  
- **P-Value:** 0.0  

The permutation test revealed that the observed difference in MAE is statistically significant, as the p-value is less than the chosen significance level of 0.05. The histogram below illustrates the distribution of permutation test statistics under the null hypothesis, with the observed difference marked by the red dashed line.

<iframe
  src="assets/Fairness_Permutation_Test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Conclusion
The results suggest that the model performs slightly better for one group of recipes than the other. Specifically, the significant difference in MAE between Quick Recipes and Time-Consuming Recipes indicates potential unfairness in the model. While the magnitude of the difference may seem small, this finding is important as it highlights areas where the model could be improved to ensure equitable performance across all recipe types.

---

### Final Thoughts
This project combined robust data cleaning, exploratory analysis, predictive modeling, and fairness assessments to uncover insights into recipe ratings and user preferences. While the final model demonstrated an improvement over the baseline, the fairness analysis identified areas for refinement, emphasizing the importance of equitable model performance. Future work could include:
- Incorporating additional features or user-specific data to enhance predictions.
- Applying advanced fairness-aware modeling techniques to reduce disparities across groups.
- Expanding the analysis to include other recipe characteristics that may influence ratings.

Through this project, we not only explored the fascinating intersection of data science and culinary arts but also underscored the ethical considerations in predictive modeling. These insights are valuable not only for understanding recipe preferences but also for designing better, more equitable models in any domain.

### Conclusion
The baseline model serves as a strong foundation for predicting `average_rating`, achieving a respectable MAE of **0.4891**. While the model effectively integrates diverse features, there is substantial room for improvement. Through hyperparameter tuning, advanced feature engineering, and interaction modeling, the predictive performance can be further enhanced. This baseline sets the stage for the final model to demonstrate significant improvements.
