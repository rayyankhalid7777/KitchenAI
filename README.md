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

