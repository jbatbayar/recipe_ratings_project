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

The the most relevant column for this investigation is definitely the `calories (#)` value in the `nutrition` column. I disregarded all the remaining values from the column. Other relevant columns would be `rating` and `avg_rating`.  

---
## Data Cleaning and Exploratory Data Analysis
I took a few steps to clean the data I have:

1. Left merged `recipes` dataset with `interactions` on `'id'` (`recipes`) and `'recipe_id'` for `interactions`.
Merged dataset now has 234429 rows with 17 columns.

2. Checked if the data types of columns in the merged dataset are suitable.
   
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

3. Filled ratings of 0 with np.nan as ratings range from 1 to 5.
We would be able to get a good 'avg_rating' estimate as it will ignore np.Nan as default when calculating. If ratings were kept as 0s, it would have affected the average rating for a recipe when it shouldn't have been included.

4. Found avg_rating per recipe, as a Series

5. Added `'avg_rating'` Series back to the dataset by merging.

6. Converted `'submitted'` and  `'date'`, which were objects into datetime.

7. Turned `'nutrition'` column from string into list.

8. Created `'calories'` column in the dataset, taking the first element of each record in the `'nutrition'` column.

As a result of these 8 data cleaning steps, the dataframe now has 234429 rows and 19 columns, with each column in a most appropriate data type. Here are the first 5 rows of my cleaned dataset with the most relevant columns for my question:

| name | id |  minutes  |  submitted  |   n_steps |   n_ingredients |   rating |   avg_rating |   calories |
|:-------------------------------------|-------:|----------:|:--------------------|----------:|----------------:|---------:|-------------:|-----------:|
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
|----------------:|---------:|---------:|---------:|---------:|----------:|
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



---
## Hypothesis Testing



---
## Framing a Prediction Problem



---
## Baseline Model



---
## Final Model



---
## Fairness Analysis


