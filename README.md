## Reviewing the relationship between Rating and the Amount of Calories of Recipes
By Jessica Batbayar(jbatbayar@ucsd.edu)

---
## Introduction

In this data science project, we delve into the intricate relationship between the rating of recipes and their caloric content. By analyzing a diverse dataset of culinary creations, our goal is to uncover patterns and correlations that may exist between the popularity or rating of a recipe and its nutritional value, specifically the number of calories. Through this exploration, we aim to provide insights that can inform healthier recipe choices while still appealing to taste preferences, potentially aiding in better dietary decisions and contributing to the broader understanding of how caloric content impacts culinary appreciation.

One common observation is that the foods we often crave and repeatedly enjoy tend to be those with higher calorie content. Personally, I have noticed that the recipes I find most satisfying and return to frequently are typically richer in calories compared to what I eat normally. 
This subjective experience raises an interesting hypothesis—perhaps the pleasure derived from high-calorie foods translates into higher ratings for such recipes on a broader scale.

We start with 2 different datasets:

Dataset 'recipes' has 83782 rows with 12 columns, each row containing information on a unique recipe. The columns are as follows:
| Column	| Description |
| --------- | ------------|
| 'name'	| Recipe name |
| 'id'	| Recipe ID |
|'minutes'	| Minutes to prepare recipe |
|'contributor_id'	| User ID who submitted this recipe |
|'submitted'	| Date recipe was submitted |
| 'tags'	| Food.com tags for recipe |
| 'nutrition'	| Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps'	| Number of steps in recipe |
| 'steps'	| Text for recipe steps, in order |
| 'description'	| User-provided description |

Dataset `interactions` contains 731927 rows with 5 columns, each row containing a review and rating by users for a recipe.
| Column	| Description |
| --------- | ------------|
| 'user_id'	| User ID |
| 'recipe_id'	| Recipe ID |
|'date'	| Date of interaction |
|'rating'	| Rating given |
| `review` | Review text |

This project delves into these datasets to investigate a compelling question: **Do people tend to rate high-calorie recipes more favorably than their low-calorie counterparts?** This is a significant question because while recipes high in calories might be preferred over low-calorie recipes by a lot of people, the health concern the choices bring is worth considering.

The the most relevant column for this investigation is definitely the `calories (#)` value in the `nutrition` column. I disregarded all the remaining values from the column. Other relevant columns would be `rating` and `avg_rating`.  

---
## Data Cleaning and Exploratory Data Analysis



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


