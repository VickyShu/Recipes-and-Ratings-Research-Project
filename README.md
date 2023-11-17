# Recipes and Ratings Research Project

**Authors**: Xin Shu, Chang Shu

## Project Overview
This project aims to explore the potential correlation between the calorie content of recipes and the ratings giving by people. This is a project for DSC80 at UCSD. The dataset used to investigate this topic can be find [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).

## Introduction
In the culinary world, the balance between taste and health is a subject of much interest and debate. Calorie content, a measure of energy intake, is a critical factor in this balance, directly affecting an individual's health and well-being. High-calorie recipes are often associated with comfort and indulgence, potentially leading to higher ratings due to satisfaction and taste. In this project, we aim to examine the relationship between recipe ratings and calorie content to reveal the preferences and trends that drive our dietary habits. However, nowadays, the modern health movement encourages lower-calorie options to combat the rising tide of lifestyle-related illnesses. Thus, exploring this relationship is not just an academic exercise, which is also a way to combine culinary arts with nutritional science for a healthier society.

### Introduction to Datasets
Our two datasets comprise detailed records from a popular cooking website, capturing the essence of what makes a recipe resonate with their audience. Within these two datasets, we are trying to answer a question: Is there a relationship between the calorie content of a recipe and its user rating?

The "RAW_recipes" dataset consists of 83,782 rows, each row representing a unique recipe. There are ten columns in RAW_recipes. Relevant columns in this dataset include:

| Column | Description |
|--------|-------------|
| 'name' | Recipe name |
| 'id' | Recipe ID |
| 'minutes' | Minutes to prepare recipe |
| 'contributor_id' | User ID who submitted this recipe |
| 'submitted'	| Date recipe was submitted |
| 'tags' | Food.com tags for recipe |
| 'nutrition'	| Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps'	| Number of steps in recipe |
| 'steps'	| Text for recipe steps, in order |
| 'description'	| User-provided description |

The "RAW_interactions" dataset is considerably larger, with 731,927 rows. This dataset contains the following columns relevant to our question:

| Column | Description |
|--------|-------------|
| 'user_id' | User ID |
| 'recipe_id'	| Recipe ID |
| 'date' | Date of interaction |
| 'rating' | Rating given |
| 'review' | Review text |

For our investigation into the connection between recipe ratings and calorie content, we use some specific columns from two datasets. From the first dataset, "RAW_recipes," we concentrated on the 'nutrition' column, which lists critical nutritional values such as calories, total fat, sugar, protein, saturated fat, and carbohydrates. We will separate the calorie content, creating a new column named ‘calories (#)'. This new column will be the cornerstone of our study as it provides a direct measure of the energy content in each recipe.

We turned to the ‘rating’ column from the second dataset, "RAW_interactions". This column provides a direct expression of user satisfaction and preference, defined as categorical values.    It is a crucial metric that reflects the collective judgment of users, which when analyzed with the calorie information, could reveal whether there's a preference trend towards higher or lower-calorie dishes.

---

## Cleaning and EDA
### Data Cleaning
Before conducting the analysis based on these two datasets, we follow the following steps to clean the datasets:

1. **Left merge the recipes and interactions datasets together:** We combine the recipes and ratings datasets using a `left` merge based on `id` and `recipe_id`. This is done to ensure all recipes are included in our analysis, even if they haven't been rated. 

2. **Fill all ratings of 0 with `np.nan`:** In the merged dataset, we convert all ratings of 0 to N/A. This is reasonable since the rationale that a rating of 0 could potentially represent cases where a rating was not given, rather than a deliberate rating of zero. Rating scales typically start from 1, and a 0 is often not a selectable option for users. Thus, a 0 could be replaced by N/A for missing or unreported ratings.

3. **Calculating average rating:** After addressing the zero ratings and merging two datasets, we calculate the average rating for each recipe by add a new column `ave_rating`.

4. **Converting `nutrition` data from string to a list of floats and assign to individual columns:** The `nutrition` column looks like a list but is actually a text string. We convert this string into a list of floats and create separate columns for each nutrient - `"[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]"` - to make the data clear and ready for analysis.

The cleaned dataframe is shown below (with only relevant columns).

| name                                 |     id |   n_steps | steps                                                             | ingredients                         |   n_ingredients |   calories |   ratings |   minutes |
|:-------------------------------------|-------:|----------:|:------------------------------------------------------------------|:------------------------------------|----------------:|-----------:|----------:|----------:|
| 1 brownies in the world best ever | 333281 |        10 | ['heat the oven to 350f and arrange the rack in the middle', ...] | ['bittersweet chocolate', ...]      |               9 |      138.4 |         4.0 |         40 |
| 1 in canada chocolate chip cookies   | 453467 |        12 | ['pre-heat oven the 350 degrees f', ...]                          | ['white sugar', 'brown sugar', ...] |              11 |      595.1 |         5.0 |         45 |
| 412 broccoli casserole               | 306168 |         6 | ['preheat oven to 350 degrees', ...]                              | ['frozen broccoli cuts', ...]       |               9 |      194.8 |         5.0 |         40 |
| 412 broccoli casserole               | 306168 |         6 | ['preheat oven to 350 degrees', ...]                              | ['frozen broccoli cuts', ...]       |               9 |      194.8 |         5.0 |         40 |
| 412 broccoli casserole               | 306168 |         6 | ['preheat oven to 350 degrees', ...]                              | ['frozen broccoli cuts', ...]       |               9 |      194.8 |         5.0 |         40 |

The type of each columns in the cleaned dataframe is shown below:
```
name              object
id                 int64
n_steps            int64
steps             object
ingredients       object
n_ingredients      int64
calories (#)     float64
rating           float64
minutes            int64
dtype: object
```
### Univariate Analysis
In the univariate analysis section, we will analyze the distribution of rating scale and the distribution of calories.
#### Distribution of Ratings Scale
<iframe src="assets/rating.html" width=800 height=600 frameBorder=0></iframe>
The plot displays a bar chart that illustrates the distribution of ratings on a 1-5 scale. The x-axis represents the rating scale with values from 1 to 5. The y-axis represents the count of ratings for each recipe.

The chart shows a clear trend: the highest rating (5) has been given significantly more often than the lower ratings of 1 through 4. This could suggest a tendency for users to favor recipes with higher ratings, or it could be an indication of a positive skew in user behavior, where users are more likely to rate a recipe if they found it very satisfactory (rating of 5).


#### Distribution of calories
<iframe src="assets/calories.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of calories for a collection of recipes, with the data filtered to exclude the top 1% of recipes with the highest calorie content.

From the histogram, we can observe the following:

**Skewed Distribution:** The distribution of calories is right-skewed, meaning there are more recipes with lower calorie content and fewer recipes with higher calorie content.

**Most Common Range:** The majority of the recipes appear to fall within the lower calorie range, with a steep drop-off as calorie content increases.

**High-Calorie Recipes:** While there are recipes with higher calorie counts, they are significantly less common, as evidenced by the long tail extending to the right of the histogram.

### Bivariate Analysis
To do the Bivariate Analysis, we will use a boxplot and a scatter plot to illustrate the relationship between the calorie content of recipes and the ratings giving by people.

#### Distribution of scatter plot for calories and the number of steps
<iframe src="assets/scatter.html" width=800 height=600 frameBorder=0></iframe>
From this visualization, it is observed that there is no strong correlation between calories and the number of steps. We could say that there is a weak relationship between the calorie content of recipes and the number of steps.

#### Distribution of box plot for calories and ratings
<iframe src="assets/boxplot.html" width=800 height=600 frameBorder=0></iframe>
This box plot displays the distribution of calorie content across different recipe ratings from 1 to 5.

The **median calorie content** appears relatively consistent across ratings, suggesting that the central tendency of calories is not strongly affected by how users rate the recipes.

The **range and interquartile range (IQR)** of calories seem similar across ratings, indicating that there is a similar variation in calorie content regardless of the rating.

Thus, from this plot, we cannot conclude anything about the relationship between the calories and ratings, as the central tendencies and spreads are similar across all rating categories.

### Interesting Aggregates
In the aggregates analysis, we will study the calories content.

| rating | mean       | median | std        | max      |
|--------|------------|--------|------------|----------|
| 1.0    | 486.595401 | 316.0  | 757.480650 | 17551.6  |
| 2.0    | 446.598226 | 326.3  | 526.058036 | 7585.0   |
| 3.0    | 425.790714 | 309.6  | 547.385352 | 13101.5  |
| 4.0    | 405.047340 | 302.0  | 487.186744 | 16894.9  |
| 5.0    | 415.213166 | 298.2  | 580.018040 | 45609.0  |

<iframe src="assets/max.html" width=800 height=600 frameBorder=0></iframe>

<iframe src="assets/withoutmax.html" width=800 height=600 frameBorder=0></iframe>
---

## Assessment of Missingness
### NMAR Analysis

---

## Hypothesis Testing


---