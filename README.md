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

** Insert Image Grouped_Table_Recipe_by_no_of_ingredients Here **

#### Pivot Table: Calories and Ratings by Number of Steps
This pivot table examines the average calories and average ratings of recipes based on the number of steps required. It provides insight into the complexity of recipes and their nutritional content, showing how the number of steps correlates with user preferences and calorie counts.

** Insert Image Pivot_Table_Calories_and_Ratings_by_no_of_steps Here **

