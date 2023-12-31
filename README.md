# Recipes and Ratings Research Project

**Authors**: Xin Shu, Chang Shu

## Project Overview
This project aims to explore the potential correlation between the calorie content of recipes and the ratings giving by people and to predict that ratings based on several features. This is a project for DSC80 at UCSD. The dataset used to investigate this topic can be find [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).

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

For our investigation into the connection between recipe ratings and calorie content, we use some specific columns from two datasets. From the first dataset, "RAW_recipes," we concentrated on the `nutrition` column, which lists critical nutritional values such as calories, total fat, sugar, protein, saturated fat, and carbohydrates. We will extract the calorie content, creating a new column named `calories (#)`. This new column will be the cornerstone of our study as it provides a direct measure of the energy content in each recipe.

We turned to the `rating` column from the second dataset, "RAW_interactions". This column could reveal whether there is a preference trend towards higher or lower-calorie dishes when analyzed with the calorie information.

---

## Cleaning and EDA
### Data Cleaning
Before conducting the analysis based on these two datasets, we follow the following steps to clean the datasets:

1. **Left merge the recipes and interactions datasets together:** We combine the recipes and ratings datasets using a `left` merge based on `id` and `recipe_id`. This is done to ensure all recipes are included in our analysis. 

2. **Fill all ratings of 0 with `np.nan`:** In the merged dataset, we convert all ratings of 0 to N/A. This is reasonable since the rationale that a rating of 0 could potentially represent cases where a rating was not given, rather than a deliberate rating of zero. Rating scales typically start from 1, and a 0 is often not a selectable option for users. Thus, a 0 could be replaced by N/A for missing or unreported ratings.

3. **Calculating average rating:** After addressing the zero ratings and merging two datasets, we calculate the average rating for each recipe by add a new column `average_rating`.

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

The chart shows a clear trend: the highest rating (5) has been given significantly more often than the lower ratings of 1 through 4. This could suggest that users tend to favor recipes with higher ratings, or it could indicate that there is a positive skew in user behavior, where users are more likely to rate a recipe if they found it very satisfactory (rating of 5).


#### Distribution of calories
<iframe src="assets/calories.html" width=800 height=600 frameBorder=0></iframe>

The histogram shows the distribution of calories for a collection of recipes, with the data filtered to exclude the top 1% of recipes with the highest calorie content.

From the histogram, we can observe the following:

**Skewed Distribution:** The distribution of calories is right-skewed, meaning there are more recipes with lower calorie content and fewer recipes with higher calorie content.

**Most Common Range:** The majority of the recipes appear to fall within the lower calorie range, with a steep drop-off as calorie content increases.

**High-Calorie Recipes:** While there are recipes with higher calorie counts, they are significantly less common, as evidenced by the long tail extending to the right of the histogram.

### Bivariate Analysis
To do the Bivariate Analysis, we will use a scatter plot to illustrate the relationship between the calorie content of recipes and the number of steps in recipe and use a boxplot to illustrate the relationship between the calorie content of recipes and the ratings giving by people.

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

The pivot table summarizes the calorie content of recipes by their user ratings. Each row corresponds to a rating value, with the following statistical measures:

**Mean:** The average calorie content for recipes with that rating.

**Median:** The middle value in the range of calorie contents for recipes with that rating.

**Standard Deviation (std):** A measure of the variation of the calorie amounts from the mean for recipes with that rating.

**Maximum (max):** The highest calorie content found in recipes for each rating category.


| rating | mean       | median | std        | max      |
|--------|------------|--------|------------|----------|
| 1.0    | 486.595401 | 316.0  | 757.480650 | 17551.6  |
| 2.0    | 446.598226 | 326.3  | 526.058036 | 7585.0   |
| 3.0    | 425.790714 | 309.6  | 547.385352 | 13101.5  |
| 4.0    | 405.047340 | 302.0  | 487.186744 | 16894.9  |
| 5.0    | 415.213166 | 298.2  | 580.018040 | 45609.0  |

Below is the plot for the pivot table.
<iframe src="assets/max.html" width=800 height=600 frameBorder=0></iframe>
From this plot we can see that the `max` values were disproportionately higher than the other statistics. It skewed the visual representation, making it challenging to observe the trends and patterns of the other variables. To address this, the `max` variable was excluded in the next plot to focus on the `mean`, `median`, and `std` variables.

<iframe src="assets/withoutmax.html" width=800 height=600 frameBorder=0></iframe>

This exclusion allowed for a clearer and more meaningful comparison of these three measures across the rating scale from 1 to 5.  From the revised plot, an interesting finding emerged: while the `mean` and `median` values show a slight decrease as ratings increase from 1 to 4, they increase again for recipes with a rating of 5. This suggests that the highest-rated recipes do not necessarily have lower calorie content. Additionally, the `standard deviation` remains relatively consistent across ratings, indicating a similar variability in calorie content regardless of the rating.  This view of the data underscores that user preferences for recipes, as reflected by ratings, may not be solely influenced by calorie content.

---

## Assessment of Missingness
### NMAR Analysis
In the NMAR scenario, the reason behind the missing data is related to the missing data itself. In the merged dataframe, the missingness type of the `description` column is probably NMAR. For example, users might not include a description for recipes that are simple or well-known because they think it's self-explanatory, or they might omit descriptions for recipes that are family secrets or unique concoctions they prefer not to share. Both cases can make its missingness be NMAR.

### Missingness Dependency
To better understand the missingness dependency, we will focus on the `rating` column of our merged DataFrame, we would like to investigate whether the missingness is dependent on other variables.  Specifically, we will conduct permutation tests on the `minutes` (time to prepare a recipe) and `n_ingredients` (number of ingredients in a recipe) columns to test for any dependency between these factors and the likelihood of a recipe being rated.

#### Rating and Minutes
- **Null Hypothesis:** The missingness of ratings is independent of the preparation time of the recipes. This means that the length of time to prepare a recipe does not affect whether a rating is missing or not.

- **Alternative Hypothesis**: The missingness of ratings depends on the preparation time of the recipes. This implies that the time to prepare a recipe does affect the likelihood of a rating being missing.

**Test Statistic:** The test statistic will be the absolute difference in mean minutes between the group of recipes with ratings and the group without ratings.

<iframe src="assets/minutes.html" width=800 height=600 frameBorder=0></iframe>

From the above plot, we notice that the distributions of minutes with ratings and minutes without ratings are quite similar, and we will conduct the permutation test to see the absolute difference mean within these two groups.

```
missing
False    103.489569
True     154.941939
Name: minutes, dtype: float64
```
We calculated the observed difference to be 51.45 (rounded to 2 decimal places). Then we conduct the permutation test, shuffle the preparation time data 1000 times, re-split it into two groups and calculate the absolute mean. The plot below shows the empirical distribution of our permuted test statistics in 1000 permutations, the red line indicates the observed test statistics.
<iframe src="assets/minutes_empirical.html" width=800 height=600 frameBorder=0></iframe>

We calculate our p-value is 0.11. Since the calculated p-value of 0.11 is greater than the significance level α of 0.05, we fail to reject the null hypothesis. This suggests that there is not enough evidence to conclude that the missingness of ratings is related to the preparation time of the recipes. 

**Therefore, we conclude that the missingness of ratings is independent of the preparation time based on our current analysis.**

#### Rating and N_ingredients (MAR)

Similarly, we want to know whether there is a significant difference in the mean number of ingredients (n_ingredients) for recipes with the missingness of rating.
- **Null Hypothesis:** The missingness of rating does not depend on the number of ingredients.

- **Alternative Hypothesis:** The missingness of rating depends on the number of ingredients.

**Test Statistic:** The test statistic will be the absolute difference in mean number of ingredients (n_ingredients) between the group of recipes with ratings and the group without ratings.

<iframe src="assets/n_ingredients.html" width=800 height=600 frameBorder=0></iframe>

From the histogram above, we notice their distributions of the number of ingredients with ratings and that of without ratings are very similar.

```
missing
False    9.061196
True     9.221934
Name: n_ingredients, dtype: float64
```

We calculated the observed difference to be 0.16 (rounded to 2 decimal places). Then we conduct the permutation test, shuffle the number of ingredients data 1000 times, re-split it into two groups and calculate the abosulte mean.

<iframe src="assets/n_ingredients_empirical.html" width=800 height=600 frameBorder=0></iframe>

The above plot shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics (0.1607379066254797). <br>
From this plot and the result of our permutation test, we get a p-value of 0.0 which is significantly less than our significance level of 5%. Therefore, we reject the null hypothesis.

---

## Hypothesis Testing
Back to our question: examine the relationship between recipe ratings and calorie content to see if higher rating corresponds to the higher calories.<br>
For this part, we define the recipe with a rating of 4 or 5 as “high”. The other with a rating of 1, 2, or 3 as “low” to calculate the mean of the calories content in these two levels.

### Permutation Test
- **Null Hypothesis:** There is no difference in mean calorie content between recipes with high ratings (defined as a rating of 4 or 5) and recipes with low ratings (defined as a rating of 1, 2, or 3).
- **Alternative Hypothesis:** There is a difference in mean calorie content, and specifically that recipes with high ratings have a different mean calorie content than recipes with low ratings. 
- **Test Statistic:** The test statistic is the absolute difference in the mean calorie counts between the `high` rating group (ratings of 4 and 5) and the `low` rating group (ratings of 1, 2, and 3).
- **Significance Level:** To ensure the accuracy of our conclusion, we decided to use a significance level of 5% as our significance level.

Below is the table that contains some columns from our dataset. We add one more column `high_rating` to our dataset, which is `High` if the rating of the recipe is 4 or 5 and `Low` if the rating is 1, 2 or 3.

| name                             | id    | calories (#) | rating | high_rating |
|----------------------------------|-------|--------------|--------|-------------|
| 1 brownies in the world best ever | 333281 | 138.4        | 4.0    | High        |
| 1 in canada chocolate chip cookies | 453467 | 595.1        | 5.0    | High        |
| 412 broccoli casserole           | 306168 | 194.8        | 5.0    | High        |
| 412 broccoli casserole           | 306168 | 194.8        | 5.0    | High        |
| 412 broccoli casserole           | 306168 | 194.8        | 5.0    | High        |

From the plot below, we notice that the distributions of different ratings and calories content are quite similar, and we will conduct the permutation test to see the absolute difference mean within these two groups.

<iframe src="assets/per_calories.html" width=800 height=600 frameBorder=0></iframe>

We conduct the permutation test, shuffle the `high_rating` column 1000 times, re-split it into two groups and calculate the absolute mean. The plot below shows the empirical distribution of our permuted test statistics in 1000 permutations, the red line indicates the observed test statistics which is 52.51 (rounded to 2 decimal places).
<iframe src="assets/calories_empirical.html" width=800 height=600 frameBorder=0></iframe>
From the plot above and the result of our permutation test, we get a p-value of 0.0 which is significantly less than our significance level of 5%. Therefore, we reject the null hypothesis.

---

## Conclusion
The permutation test shows a statistically significant difference in the mean calorie content between recipes with high ratings (ratings of 4 and 5) and those with low ratings (ratings of 1, 2, and 3). This result suggests that the calorie content of a recipe is indeed associated with its user rating. It indicates that recipes with higher ratings tend to have different calorie content compared to those with lower ratings. This finding highlights a potential preference or trend among users related to the calorie content of highly rated recipes.

---

## Recipes Rating Prediction

## Framing the Problem
In the culinary world, the balance between taste and health is a subject of much interest and debate. Meals are a critical factor in this balance, directly affecting an individual's health and well-being. In this project, we aim to predict the rating of a recipe based on various features, like average rating of a recipe, number of ratings for each recipe, users' average rating for recipes, and minutes to prepare a recipe. Therefore, we will build a regression model to fit our data to predict the rating for unseen recipes.

#### **Response Variable**
We chose `rating` as the response variable, which has five different classes from star one to star five. Understanding the rating of a recipe helps people to regulate their meals for better health.

#### **Evaluation Metric**
To evaluate the model’s performance, we will use metrics such as **RMSE**. That’s because there are lots of outliers in our features, like lots of time to prepare recipes. So we choose RMSE because it is sensitive to outliers. It will give a higher error value, signaling that these points are not being predicted accurately.

---

## Baseline Model

### Model Description
In our baseline model, we intend to predict whether the `rating` is based on the average rating of the recipe and total minutes needed to prepare for a recipe. We will use `recipe_average_rating` and `log_minute` as features of our baseline Linear Regression model.

### Feature Transformations
For this baseline model, we did not have any categorical features, so we did not need to use OneHotEncoder() or any other categorical transformations. 

`recipe_average_rating`: This is an ordinary feature. From the scatter plot, we can see that there is some relationship between the average rating and the rating, which is that higher average ratings tend to have higher ratings. We just keep it as the raw integer value for simplicity.

<iframe src="assets/fig_average_rating.html" width=800 height=600 frameBorder=0></iframe>

`minutes`: This is a nominal feature. First we plot the box plot below to see the relationship between minutes. We found that there are lots of recipes that have a long preparation time, which represents the outliers within each rating level. So we decided to use log transformation to transform minutes by log to make it more suitable for prediction. From the new scatter plot, we can see that the longest preparation time is around 15.

<iframe src="assets/fig_minutes_rating.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/fig_log_min.html" width=800 height=600 frameBorder=0></iframe> 

### Performance
Our model achieved a training RMSE of 0.3715 and a testing RMSE of 0.5147. This level of RMSE indicates that the model has a reasonably good predictive capability. That is, if a recipe has a high average rating, it is more likely to receive similarly high ratings from new reviewers, keeping its high score. Our model's performance corroborates this theory, following the tendency of recipes with higher ratings to maintain higher future ratings.

| Metric | Train RMSE | Test RMSE |
|--------|-------------|-------------|
| 'RMSE' | 0.371539447765363 | 0.5147232717849082 |

---

## Final Model

### **Description**

`recipe_num_ratings`: For the number of ratings for each recipe, we want to study whether a recipe with a higher number of ratings possibly tends to receive higher ratings from people. 

`user_average_rating`: For the user's average rating, we want to check if a person tends to give a higher rating just because he habitually gives high scores. 

### **Performance**

In crafting our predictive model, we incorporated two additional features: `user_average_rating` and `recipe_num_ratings`. The inclusion of `user_average_rating` is predicated on the assumption that individual rating behaviors are consistent across multiple reviews. Thus, a user who tends to give higher ratings might similarly rate new recipes they encounter with high ratings. This feature captures the individual bias of users, which is fundamental to understanding the variability in recipe ratings.

The second feature, `recipe_num_ratings`, is grounded in the theory of wisdom of the crowds, which states that the information collection in groups results in decisions that are often better than the decisions made by any single member of the group. This implies that recipes with a large number of ratings are likely to have a more reliable rating, indicating that a large number of ratings on one recipe tends to have higher ratings than those with a smaller number of ratings.

For our predictive modeling, we also tried the Decision Tree Regressor model, a non-linear model that is well-suited for capturing complex patterns in data which linear models might miss. Decision trees are particularly adept at modeling interactions between different features, which we hypothesized would be present in our dataset.

Then we fine-tuned our model using GridSearch to optimize the hyperparameters `max_depth` and `min_samples_split`. This approach systematically works through multiple combinations of parameter tunes, cross-validating as it goes to determine which tune gives the best performance. We evaluated 70 different combinations, seeking to strike a balance between a model that is complex enough to learn the data well, but not so complex that it overfits. Finally, we got the best performance with the `max_depth` of 10 and the `min_samples_split` of 100.

Next, we calculated and compared the **RMSE** of the testing sets for each model as the table shown below. We found the DecisionTreeRegressor with the best hyperparameter has the lowest RMSE, so we chose this to be our final model.

| Metric | Test RMSE |
|--------|-------------|
| 'RMSE for regression with two features' | 0.5147232717849082 |
| 'RMSE for regression with three features' | 0.3758448690196555 |
| 'RMSE for regression with four features' | 0.3758027040111092 |
| 'RMSE for the DecisionTreeRegressor' | 0.43897293968980095 |
| 'RMSE for the DecisionTreeRegressor with the best hyperparameter' | 0.3215798723839845 |

<iframe src="assets/fig.html" width=800 height=600 frameBorder=0></iframe>
As we can see from the plot below, our final model's performance was an improvement over our baseline model, which we can infer our baseline model was a simpler and a basic linear regression model without these additional features or a non-tuned decision tree. The final Decision Tree Regressor with optimal hyperparameters yielded the lowest RMSE on the testing set, indicating that it was the most accurate at predicting recipe ratings. The reduction in RMSE from the baseline to the final model signifies an enhancement in predictive accuracy, likely due to the model's increased complexity and ability to capture more subtle patterns within the data.

---

## Fairness Analysis

In our pursuit of developing a fair and equitable predictive model for recipe ratings, we pose a critical question: Does our model demonstrate differential performance for recipes with perfect 5-star ratings as opposed to those rated 4 stars or below?
For this question, we divided our recipes into two different groups based on its rating.

**GroupX:** Recipes with 5-star ratings.

**GroupY:** Recipes with ratings of 4 stars or below.

### Permutation Test
- **Null Hypothesis:** Our model is fair. Its prediction of recipes with 5 stars and the recipes with four or less stars are roughly the same, and any differences are likely due to chance.
- **Alternative Hypothesis:** Our model is not fair. Its prediction of recipes with 5 stars is different from recipes with 4 or less stars.
- **Test Statistic:** The test statistic is the absolute difference in the RMSE between these two groups: high rating group (ratings of 5) and the low rating group (ratings below 5).
- **Significance Level:** To ensure the accuracy of our conclusion, we decided to use a significance level of 5% as our significance level.

We conduct the permutation test, shuffle the recipes with 5 stars column 1000 times, re-split it into two groups and calculate the absolute differece in RMSE. The plot below shows the empirical distribution of our permuted test statistics in 1000 permutations, the red line indicates the observed test statistics which is 0.2197 (rounded to 4 decimal places) which is our RMSE.

<iframe src="assets/fig_calories_empirical.html" width=800 height=600 frameBorder=0></iframe>

We calculate our p-value is 0. Since the calculated p-value of 0 is smaller than the significance level α of 0.05, we reject the null hypothesis. We cannot say that our model is fair based on our RMSE fairness analysis alone. 

Therefore, we conclude that our model is not fair. Its prediction of recipes with 5 stars is different from recipes with 4 or less stars.

---