## Reviewing the relationship between Rating and the Amount of Calories of Recipes
By Jessica Batbayar(jbatbayar@ucsd.edu)

---
## Introduction

In this data science project, we delve into the intricate relationship between the rating of recipes and their caloric content. By analyzing a diverse dataset of culinary creations, our goal is to uncover patterns and correlations that may exist between the popularity or rating of a recipe and its nutritional value, specifically the number of calories. Through this exploration, we aim to provide insights that can inform healthier recipe choices while still appealing to taste preferences, potentially aiding in better dietary decisions and contributing to the broader understanding of how caloric content impacts culinary appreciation.

One common observation is that the foods we often crave and repeatedly enjoy tend to be those with higher calorie content. Personally, I have noticed that the recipes I find most satisfying and return to frequently are typically richer in calories compared to what I eat normally. 
This subjective experience raises an interesting hypothesis—perhaps the pleasure derived from high-calorie foods translates into higher ratings for such recipes on a broader scale.

We start with 2 different datasets:

Dataset `recipes` has 83782 rows with 12 columns, each row containing information on a unique recipe. The columns are as follows:

| Column	| Description |
| :--------- | :------------|
| `'name'`	| Recipe name |
| `'id'`	| Recipe ID |
| `'minutes'`	| Minutes to prepare recipe |
| `'contributor_id'`	| User ID who submitted this recipe |
| `'submitted'`	| Date recipe was submitted |
| `'tags'`	| Food.com tags for recipe |
| `'nutrition'`	| Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`	| Number of steps in recipe |
| `'steps'`	| Text for recipe steps, in order |
| `'description'`	| User-provided description |
| `'ingredients'` | Ingredients used for recipe |
| `'n_ingredients'` | Number of ingredients in recipe |



Dataset `interactions` contains 731927 rows with 5 columns, each row containing a review and rating by users for a recipe.

| Column	| Description |
| :--------- | :------------|
| `'user_id'`	| User ID |
| `'recipe_id'`	| Recipe ID |
|`'date'`	| Date of interaction |
|`'rating'`	| Rating given |
| `'review'` | Review text |


This project delves into these datasets to investigate a compelling question: **Do people tend to rate high-calorie recipes more favorably than their low-calorie counterparts?** This is a significant question because while recipes high in calories might be preferred over low-calorie recipes by a lot of people, the health concern the choices bring is worth considering.

The the most relevant column for this investigation is definitely the `'calories (#)'` value in the `'nutrition'` column. I disregarded all the remaining values from the column. Other relevant columns would be `'rating'` and `'avg_rating'`.  

---
## Data Cleaning and Exploratory Data Analysis
I took a few steps to clean the data I have:

1. Left merged `recipes` dataset with `interactions` on `'id'` (`recipes`) and `'recipe_id'` for `interactions`.
   Merged dataset now has 234429 rows with 17 columns.

2. Checked if the data types of columns in the merged dataset are suitable.
   Here are the data types for all columns:
   
   | Column	| Description |
| :-------- | :----------- |
| `'name'`	| object |
| `'id'`	| int64 |
| `'minutes'`	| int64 |
| `'contributor_id'`	| int64 |
| `'submitted'`	| object |
| `'tags'`	| object |
| `'nutrition'`	| object |
| `'n_steps'`	| int64 |
| `'steps'`	| object |
| `'description'`	| object |
| `'ingredients'` | object |
| `'n_ingredients'` | int64 |
| `'user_id'`	| float64 |
| `'recipe_id'`	| float64 |
|`'date'`	| object |
|`'rating'`	| float64 |
| `'review'` | object |

4. Filled ratings of 0 with np.nan as ratings range from 1 to 5.
We would be able to get a good 'avg_rating' estimate as it will ignore np.Nan as default when calculating. If ratings were kept as 0s, it would have affected the average rating for a recipe when it shouldn't have been included.

5. Found avg_rating per recipe, as a Series

6. Added `'avg_rating'` Series back to the dataset by merging.

7. Converted `'submitted'` and  `'date'`, which were objects into datetime.

8. Turned `'nutrition'` column from string into list.

9. Created `'calories'` column in the dataset, taking the first element of each record in the `'nutrition'` column.


As a result of these 8 data cleaning steps, the dataframe now has 234429 rows and 19 columns, with each column in a most appropriate data type. Here are the first 5 rows of my cleaned dataset with the most relevant columns for my question:

| name | id |  minutes  |  submitted  |   n_steps |   n_ingredients |   rating |   avg_rating |   calories |
|:-------------------------------------|:-------|:----------|:--------------------|:----------|:---------------|:---------|:------------|:-----------|
| 1 brownies in the world    best ever | 333281 |        40 | 2008-10-27 00:00:00 |        10 |               9 |        4 |            4 |      138.4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 2011-04-11 00:00:00 |        12 |              11 |        5 |            5 |      595.1 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |         6 |               9 |        5 |            5 |      194.8 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |         6 |               9 |        5 |            5 |      194.8 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |         6 |               9 |        5 |            5 |      194.8 |


### Univariate Analysis
I examined the distribution of calories in a recipe below. I put them into 6 bins so that I will be able to see if there are any extreme values in this column that might affect my analysis in the future. From the plot below, we can see that there are 954 records where the recipe has 0-5 calories and 81 records with 10000+ calories. Though not high in quantity, I think that these values can be considered outliers.

<iframe
  src="assets/calories_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
For this analysis, I looked at the relationship between calories and ratings. I used a small dataset that contained records of recipes and ratings which had 5-10000 calories. I computed the mean calories for this dataset, which was equal to 416.3725968915825, and added a new column `'more_than_mean'` which has `True` for records in which their value in `'calories'` is higher than the computed mean and `False` for those which were equal or lower than the mean. Looking at the graph below, we see that calories of recipes that were rated 1, 2, or 3 were more likely to be higher than the mean while the calories of recipes with the rating of 4 and 5 were more likely to be lower than the mean.

<iframe
  src="assets/bivar_meancals.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
I looked into the relationship between the number of ingredients a recipe requires (`'n_ingredients'`), which ranged from 1 to 37, and the amount of calories it had. The pivot table below is indexed by `'n_ingredients'`, with columns as `'rating'`, and with values as mean calories. There's a trend where the the average calories increase when the number of ingredients increase. And for the same `'n_ingredients'`, the mean amount of calories is usually higher in `'rating'` of 1 than in `'rating'`of 5.

|   n_ingredients |      1.0 |      2.0 |      3.0 |      4.0 |       5.0 |
|:---------------|---------:|---------:|---------:|---------:|----------:|
|               1 |  nan     |  199.5   |  136.05  |  758.68  |  1263.21  |
|               2 |  732.766 |  349.979 |  292.772 |  347.403 |   390.09  |
|               3 |  419.622 |  329.928 |  335.446 |  268.41  |   276.888 |
|               4 |  394.972 |  345.579 |  325.212 |  295.253 |   301.288 |
|               5 |  487.563 |  362.728 |  332.405 |  332.652 |   322.762 |
|               6 |  480.697 |  406.282 |  338.827 |  331.788 |   353.73  |
|               7 |  437.898 |  431.593 |  404.565 |  382.173 |   385.01  |
|               8 |  426.47  |  392.315 |  385.13  |  374.346 |   380.566 |
|               9 |  431.817 |  407.474 |  416.227 |  408.351 |   414.073 |
|              10 |  504.764 |  390.436 |  464.635 |  430.635 |   431.29  |
|              11 |  518.621 |  473.763 |  447.069 |  464.698 |   466.726 |
|              12 |  481.568 |  446.025 |  483.974 |  449.799 |   463.182 |
|              13 |  471.365 |  613.924 |  452.795 |  451.492 |   490.16  |
|              14 |  555.204 |  599.497 |  502.469 |  466.983 |   518.264 |
|              15 |  648.734 |  646.12  |  681.038 |  520.831 |   545.531 |
|              16 |  601.148 |  610.905 |  513.886 |  564.316 |   587.254 |
|              17 |  534.337 |  613.74  |  640.673 |  528.639 |   549.3   |
|              18 | 1383.63  | 1226.8   |  540.492 |  591.55  |   609.648 |
|              19 |  603.558 |  532.813 |  517.72  |  534.66  |   599.235 |
|              20 |  610.593 |  597.1   |  859.706 |  663.231 |   655.086 |
|              21 |  948.844 | 1074.17  |  769.953 |  832.135 |   756.979 |
|              22 | 1947.12  |  nan     |  740.025 |  791.418 |   680.999 |
|              23 |  851.6   |  149.6   |  681.883 |  635.236 |   671.108 |
|              24 |  528.9   | 1049.05  |  660.8   |  692.518 |   576.93  |
|              25 |  965.6   |  965.6   |  739.6   |  780.2   |   766.322 |
|              26 |  nan     |  nan     |  456.1   |  854.564 |   847.61  |
|              27 | 1057.9   | 1136.7   | 1514     |  701.1   |  1475.78  |
|              28 |  nan     |  nan     |  nan     |  597.15  |   689.778 |
|              29 |  nan     |  nan     |  nan     | 1011.35  |   894.596 |
|              30 |  531.2   |  nan     |  nan     |  554.1   |   697.996 |
|              31 |  nan     |  nan     |  nan     |  nan     |   872.454 |
|              32 |  nan     |  nan     |  nan     |  nan     |   864.475 |
|              33 |  nan     |  nan     |  nan     |  nan     |   338.2   |
|              37 |  nan     |  nan     |  nan     |  nan     | 10687.7   |

---
## Assessment of Missingness

### NMAR Analysis
In the dataset, columns `'description'`, `'review'`, `'rating'` had numerous values that were missing. 

I believe that `'description'` is NMAR. It is possible for a user to not include a description for their recipe because they of the complexity of it (it might have either been too simple that it is self-explanatory or too complex that they did not want to spend time writing it.

Additional information that could be helpful are `'name'`,`'n_steps'`, `'minutes'` of the recipe because we might be able to assume the complexity of the cooking process for that certain recipe.


### Missingness Dependency
For this step, I will be examining if the missingness of the `'review'` column is dependent on the `'calories'` column and the '`minutes'` column.

- Calories and Review

  I believed that whether or not a user writes a review on a recipe depends on their satisfaction with the recipe. User might write a review if their experience was extreme (either rating 1 or 5) and might choose not to write anything because their experience was nothing out of normal. This is why I chose `'avg_rating'` as a column that `'review'`'s missingness depends on.
  - ***Null Hypothesis:*** Missingness of reviews does not depend on the calories of the recipe.
  - ***Alternate Hypothesis:*** Missingness of reviews does depend on the calories of the recipe.
  - ***Test Statistic:*** Difference of means in the `'calories'` of the distribution of the group with missing reviews (`'missing_review'` == True) and the distribution of the group without missing reviews (`'missing_review'` == False).
  - ***Significance Level:*** 0.05

  Here is the table with average ratings in each group.
  
  | missing_review   |   calories |
|:-----------------|:-----------|
| False            |    419.45  |
| True             |    738.719 |

  The observed test statistic was **319.26908562595804**.

  I ran a permutation test by shuffling the missingness label of review for 1000 times to collect simulated difference in means.
  <iframe
  src="assets/missingness1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

  The p-value was 0.003, and since it is lower than our significance level 0.05, we **reject** the null hypothesis. The missingness of `'review'` does depend on `'calories'` column.
  
- Time taken and Review
  
  - ***Null Hypothesis:*** Missingness of reviews does not depend on the time taken for the recipe.
  - ***Alternate Hypothesis:*** Missingness of reviews does depend on the time taken for the recipe.
  - ***Test Statistic:*** Difference of means in the `'minutes'` of the distribution of the group with missing reviews (`'missing_review'` == True) and the distribution of the group without missing reviews (`'missing_review'` == False).
  - ***Significance Level:*** 0.05

   Here is the table with average time taken in each group:
  
  | missing_review   |   minutes |
|:-----------------|:----------|
| False            |   106.781 |
| True             |   140.345 |

The observed test statistic was **33.56346811767196**.

I ran a permutation test by shuffling the missingness label of review for 1000 times to collect simulated difference in means.
  <iframe
  src="assets/missingness2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


  The p-value was 0.092, and it is higher than our significance level 0.05, so we **fail to reject** the null hypothesis. We do not have enough evidence to reject that the missingness of `'review'` is dependent on `'minutes'`.

---
## Hypothesis Testing
I will be performing a permutation test with the following hypotheses, test statistic, and significance level:

- **Null Hypothesis:** The ratings of recipes with more and less calories than mean are from the same distibution.
- **Alternate Hypothesis:** The mean rating of recipes with calories lower than the mean is higher than the mean rating of recipes with calories greater than the mean.
- **Test Statistic:** Difference in group means (mean of ratings of recipes with calories more than average - mean of ratings of recipes with calories less than average)
- **Significance level:** 0.01

Using the `'more_than_mean'` column from the Bivariate analysis and `'rating'` from our dataset, I computed the difference in mean ratings 1000 times. The observed statistic, mean rating of recipes with calories higher than average - mean rating of recipes with calories higher than average, was -0.019303469126112027.

<iframe
  src="assets/hypo.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As a result, I had a p-value of **1.0**. This was much higher than my significance level 0.01, so I **failed to reject** my null hypothesis. I don't have sufficient evidence to conclude that users are favorable towards recipes with lower calories compared to recipes with higher calories.


---
## Framing a Prediction Problem

I plan to predict the **rating (1, 2, 3, 4, or 5) a recipe received**. This is a classification problem, and since there are more than 2 classes for the model to classify, I will be building a multi-class RandomForestClassifier.

Response variable: `'rating'`, which is an ordinal categorical variable
   Rating is something that represent the whole recipe, whether if the recipe was good or bad for a user. With a merged dataset     of recipe informations and ratings, it made sense to predict the rating a recipe might receive depending on features of the      recipe.

Evaluation metric: F1-score
I thought that we should not consider recall or precision more important than the other, that they're both equally important. In our lecture, it was said that accuracy is misleading in the presence of class balance. When I performed value_counts() on the merged dataset's `'rating'` column, 169676 out of 219393 (non-null) records were rated 5, which takes up about 77.3%. This shows that the `'rating'` class is imbalanced, and that F-1 scores are a more appropriate metric for this model.  

The information I currently possess is the merged dataset I cleaned, so all columns of this dataset could be used as features.

---
## Baseline Model

I am building a RandomForestClassifier for my baseline model. I will be using `'minutes'`, a quantitative column, and `'many_ingredients'`, a Boolean column. When I ran a .corr() on the dataset, the only column with a positive correlation with the `'rating'` column was `'minutes'`. `'many_ingredients'` is a column I made from `'n_ingredients'`, and it contains True for rows which has `'n_ingredients'` > 15 and False for those that are not. As we saw in *Interesting Aggregates* , `'n_ingredients'` ranges from 1 to 37, so 15 as a threshold seems like a suitable value. As mentioned above, the `'rating'` column contains missing values, so I performed probabilistic imputation on the column before building the model.

I kept the values of '`minutes'` feature the same while one hot encoding the categorical column '`many_ingredients'` since it's a nominal feature. 

For this model, the weighted F-1 score came out to be **0.66**.

---
## Final Model

For this final model, on top of the current 2 features, I added 2 new features: `'calories'` and `'n_steps'`. I kept `'many_ingredients'` one hot encoded, but transformed `'minutes'`.

-`'minutes'`
As mentioned before, this is the only column out of the datas I had that has positive correlation with `'rating'`, meaning that rating increased as time taken for a recipe increased. I believe that this feature would be helpful when predicting a rating of a recipe. This column has many extreme values, with 1.05 million as a value at maximum. So I used the `RobustScaler` transformer for this final model, which handles outliers effectively in numerical features.

-`'many_ingredients'`
Column has binary values depending on whether recipe used more than 15 ingredients or not. I decided to keep it the same as the baseline model, where I transformed the column by `OneHotEncoder`.

-`'calories'`
I found out that ratings tend to be higher for recipes with lower calories. Since the initial purpose of this project was to investigate the relationship between calories and ratings, I believed that the `'calories'` column could be an important feature to predict ratings. From the *Univariate analysis* above, we saw that `'calories'` has a lot of extreme values, some recipes had 10000+ calories for instance. Because of this, I used `RobustScaler` again for this feature.
 
-`'n_steps'`
With a bivariate analysis, I found out that these 2 columns have a negative correlation. Column has values up to 100, which is not that extreme (it makes sense that a recipe can take up to 100 steps), so I used the `StandardScaler` to standardize the values for this feature.

I started with a `RandomForestClassifier(max_depth = None, n_estimators=100, criterion='entropy'))`
I used `GridSearchCV` to find the best hyperparameters of `max_depth` and `min_samples_split`, and the best combination of the hyperparameters were 30 for `max_depth` and 20 for `min_samples_split`. I noticed that the lowest value of max_depth and the highest value of min_samples_split were chosen, which shows that they worked to decrease the variance of the decision tree that is high in variance.

The final weighted F-1 score came out to be **0.68**, which is an increase by 0.02 from the baseline model.

---
## Fairness Analysis

For this final step, I wanted to **check if my model performed worse for individuals where `'n_steps'` is greater or equal to 15 than it does for individuals where `'n_steps'` is lower than 15**. 

My evaluation metric the recall parity because I want to ensure that the model equally captures true high ratings (True Positives) for both of the groups and recall measures the ratio of true positives to the total number of actual positives.

False negatives (Predicted Negative, Actually Positive) would be dangerous because for a user, if they got a bad rating for a recipe as a prediction, when it was actually a high-rated one, they would avoid trying that recipe, therefore losing the opportunity to try out a good recipe. Since a high rating would implicitly mean a good recipe, labeling a recipe bad when it was not is not ideal.

- **Null Hypothesis:** This model is fair. It performs the same for groups where `'n_steps'` >= 15 and `'n_steps'` < 15.
- **Alternate Hypothesis:**: Model is unfair.
- **Test Statistic:** Difference in recall score (n_ingredients >= 15 - n_ingredients < 15)
- **Significance level:** 0.01

The observed difference in recall score was 0.018382031906093.

I performed 1000 permutations, collecting the difference in recall scores after shuffling the `'few_steps'` column.

<iframe
  src="assets/fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The p-value was 0, which is lower than my significance level of 0.01, so I **rejected** the null hypothesis. The difference in recall between the 2 groups is statistically significant.
