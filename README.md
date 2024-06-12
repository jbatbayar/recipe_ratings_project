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
I examined the distribution of calories in a recipe below. I put them into 6 bins so that I will be able to see if there are any extreme values in this column that might affect my analysis in the future.  

<iframe
  src="assets/calories_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

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


