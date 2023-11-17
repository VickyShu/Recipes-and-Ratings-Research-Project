# Recipes and Ratings Research Project

**Author**: Xin Shu, Chang Shu

## Project Overview
This project aims to explore the potential correlation between the calorie content of recipes and the ratings giving by people. This is a project for DSC80 at UCSD. The dataset used to investigate this topic can be find [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).

## Introduction

In this project, we studied the effectiveness of spice challenges in building team morale.

---

## Cleaning and EDA
### Data Cleaning
Before conducting the analysis based on these two datasets, we follow the following steps to clean the datasets:

1. **Left merge the recipes and interactions datasets together:** We combine the recipes and ratings datasets using a `left` merge based on `id` and `recipe_id`. This is done to ensure all recipes are included in our analysis, even if they haven't been rated. 

2. **Fill all ratings of 0 with `np.nan`:** In the merged dataset, we convert all ratings of 0 to N/A. This is reasonable since the rationale that a rating of 0 could potentially represent cases where a rating was not given, rather than a deliberate rating of zero. Rating scales typically start from 1, and a 0 is often not a selectable option for users. Thus, a 0 could be replaced by N/A for missing or unreported ratings.

3. **Calculating average rating:** After addressing the zero ratings and merging two datasets, we calculate the average rating for each recipe by add a new column `ave_rating`.

4. **Converting `nutrition` data from string to a list of floats and assign to individual columns:** The `nutrition` column looks like a list but is actually a text string. We convert this string into a list of floats and create separate columns for each nutrient - `"[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]"` - to make the data clear and ready for analysis.

The cleaned dataframe is shown below (with only relevant columns).

| name                                 |     id |   n_steps | steps                                                             | ingredients                         |   n_ingredients |   calories |   ratings |
|:-------------------------------------|-------:|----------:|:------------------------------------------------------------------|:------------------------------------|----------------:|-----------:|----------:|
| 1 brownies in the world    best ever | 333281 |        10 | ['heat the oven to 350f and arrange the rack in the middle', ...] | ['bittersweet chocolate', ...]      |               9 |      138.4 |         4.0 |
| 1 in canada chocolate chip cookies   | 453467 |        12 | ['pre-heat oven the 350 degrees f', ...]                          | ['white sugar', 'brown sugar', ...] |              11 |      595.1 |         5.0 |
| 412 broccoli casserole               | 306168 |         6 | ['preheat oven to 350 degrees', ...]                              | ['frozen broccoli cuts', ...]       |               9 |      194.8 |         5.0 |
| 412 broccoli casserole               | 306168 |         6 | ['preheat oven to 350 degrees', ...]                              | ['frozen broccoli cuts', ...]       |               9 |      194.8 |         5.0 |
| 412 broccoli casserole               | 306168 |         6 | ['preheat oven to 350 degrees', ...]                              | ['frozen broccoli cuts', ...]       |               9 |      194.8 |         5.0 |

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
dtype: object
```

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---