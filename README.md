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

**Conclusion**: Since the p-value is much greater than 0.05, we fail to reject the null hypothesis. The missingness of `average_rating` does not depend on the sodium content.  
**Insert Sodium Permutation Test Histogram Here**

#### Minutes and Missingness
- **Null Hypothesis**: The missingness of `average_rating` does not depend on the preparation time (`minutes`) of the recipe.
- **Alternate Hypothesis**: The missingness of `average_rating` depends on the preparation time (`minutes`) of the recipe.
- **Test Statistic**: The absolute difference in the mean preparation time (`minutes`) between recipes with missing and non-missing `average_rating` values.
- **Results**:
  - Observed Test Statistic: **117.34**
  - P-value: **0.04**

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
  src="assets/Permutation_Test_missingness_'sodium'.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

2. **`minutes` Plot**: The histogram shows that the observed statistic is significantly larger than most of the simulated test statistics, supporting dependency.  
<iframe
  src="assets/Permutation_Test_missingness_'minutes'.html"
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
