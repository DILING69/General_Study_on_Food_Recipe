# General Study on Food Recipe
## Introduction
For this project, we are working closely with two CSV files, `RAW_recipes.csv` and  `RAW_interactions.csv`. `RAW_recipes.csv` contains the information about the recipes, including:
- the recipe name
- ID of the recipe
- number of munites needed to prepare the recipe
- the user ID of the User that submitted the recipe
- the date which the user submitted the recipe
- tags for the recipe assigned by Food.com
- nutrition information of the recipe in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”
- number of steps needed to complete the recipe
- the steps of making the recipe, in order
- description of recipe provided by the user

`RAW_interactions.csv` contains the information about the interactions users left on the different recipes, including:
- user id of the user who interacted with the recipe
- recipe id of the recipe being interacted with
- date of the interaction
- the rating given to the recipe
- text review given to the recipe

These datasets are important because by working with both of them at the same time, we could extract many interesting information and perform a series of analysis that could help use better understand how people few about different recipes depending on different attibutes of them such as its calories, the number of step necessary, etc..

With these datasets, we are trying to discover the relationship between the number of calories of the recipe and the average rating that the recipe received. 
- Is the missingness of the rating related to itself (Not missing at random(NMAR))
- Is the missingness of the rating related to the number of calories of the recipe (Missing at Random(MAR))
- Does higher number of calories imply a higher rating of the recipe?

These questions are important topics to study because the answer to these questions could show us the attitude people have towards recipes of different number of calories. We could learn about people's life style and preferences in such way. 

We will use the two datasets to answer these questions. `RAW_recipes.csv` has a total of 83782 rows and `RAW_interactions.csv` has a total of 731927 rows. 

From `RAW_recipes.csv`, we will be working closely with the nutritions column. This column contains the nutrition information of the recipe such as the number of calories, total fat, etc.. We will look at the number of calories of each recipe from this column

From `RAW_interactions.csv`, we will work closely with the rating column. This column contains the rating from 1 to 5 of recipes given by different users. 

## Cleaning and EDA

### Data Cleaning
