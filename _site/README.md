# Recipes and Ratings Research Project

**Author**: Xin Shu, Chang Shu

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