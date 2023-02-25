# General Study on Food Recipe

## Introduction

For this project, we are working closely with two CSV files, `RAW_recipes.csv` and `RAW_interactions.csv`. `RAW_recipes.csv` contains the information about the recipes, including:

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

Prior to using the datasets to answer our questions, we performed a series of data cleaning to our datasets.
First, we got the the average rating of each recipe by merging the recipe df with the interaction df. After merging, we replaced 0 ratings by `NaN` because 0 rating should exist and is probably caused by a mistake in the process. If a rating is given, it should be at least one out of five. Afterwards, we grouped by `id` and took the mean of the rating column and matched it with the recipes in the recipe df by merging the two.

Then we checked the data type of each column to see if they are proper.

1. We casted `tags`,`nutrition`,`steps`, and `ingredients` columns from columns of string to columns of list that is the correct types for these columns.
2. We extracted the calories data and put into a new column named `calories (#)` to make our analysis easier later
3. We checked the unique for calories, and drop any rows with colories more than 15k. 15k calories and above from a recipe is not a reasonable, since the recommended daily calorie intake is 2000-2500 calorie
4. We dropped `steps`,`tags`,`ingredients`,and `nutrition` columns. We are not using these columns in our analysis

Below is the first five rows of the eda_df

| | name | id | minutes | contributor_id | submitted | n_steps | description | n_ingredients | average_rating | calories (#) |

| 0 | 1 brownies in the world best ever | 333281 | 40 | 985201 | 2008-10-27 | 10 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you\'ll ever make.....sereiously! there\'s no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they\'re pure heaven! | 9 | 4 | 138.4 |

| 1 | 1 in canada chocolate chip cookies | 453467 | 45 | 1848091 | 2011-04-11 | 12 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don\'t have margarine or don\'t like it, then just use butter (softened) instead. | 11 | 5 | 595.1 |

| 2 | 412 broccoli casserole | 306168 | 40 | 50969 | 2008-05-30 | 6 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | 9 | 5 | 194.8 |

| 3 | millionaire pound cake | 286009 | 120 | 461724 | 2008-02-12 | 7 | why a millionaire pound cake? because it\'s super rich! this scrumptious cake is the pride of an elderly belle from jackson, mississippi. the recipe comes from "the glory of southern cooking" by james villas. | 7 | 5 | 878.3 |

| 4 | 2000 meatloaf | 475785 | 90 | 2202916 | 2012-03-06 | 17 | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese. | 13 | 5 | 267 |'

### Univariate Analysis

This is the visualization for calories distribution. As the calories increases for dishes, the number of dishes decreases dramatically.

<iframe src="assets/distribution_of_calories_less_than_15K.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

This is the visualization for calories distribution.This scatterplot's x-axis is the average rating for the dishes, and y-axis is the calories of that dishes except for the clustering on the whole number rating. Sometimes, there is only one review that would only create whole number rating.

<iframe src="assets/scatter_of_avgRating_and_recipe_calories.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

This df is groupby the average rating category (each category is 0.5), and we calculated its avergae calories. By observing the graph, we can observe a general decreasing trend of calories as the average rating increases

<iframe src="assets/Hist_of_caloriesSum_among_Ratings.html" width=800 height=600 frameBorder=0></iframe>

This df is the source of the histogram above

<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th></th>\n      <th>rating_category</th>\n      <th>calories (#)</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>(0.996, 1.4]</td>\n      <td>447.964407</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>(1.4, 1.8]</td>\n      <td>391.865000</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>(1.8, 2.2]</td>\n      <td>462.223226</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>(2.2, 2.6]</td>\n      <td>367.801429</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>(2.6, 3.0]</td>\n      <td>434.062426</td>\n    </tr>\n    <tr>\n      <th>5</th>\n      <td>(3.0, 3.4]</td>\n      <td>476.333000</td>\n    </tr>\n    <tr>\n      <th>6</th>\n      <td>(3.4, 3.8]</td>\n      <td>440.429185</td>\n    </tr>\n    <tr>\n      <th>7</th>\n      <td>(3.8, 4.2]</td>\n      <td>423.680808</td>\n    </tr>\n    <tr>\n      <th>8</th>\n      <td>(4.2, 4.6]</td>\n      <td>414.621617</td>\n    </tr>\n    <tr>\n      <th>9</th>\n      <td>(4.6, 5.0]</td>\n      <td>422.623487</td>\n    </tr>\n  </tbody>\n</table>

## Assessment of Missingness

### NMAR Analysis

We identify the column of description as the NMAR. It is not reasonable to conclude that the missingness of description would depend on any other columns. In the data generating process, the missingness of description is occurred when the recipe owner did not enter any description when they upload the recipes, or they enter a invalid description that is prohibited by the website. In this case, the missingness of description depends on the description itself.

### Missingness Dependency

We believe the missingness of the Average rating does not depend on column minutes. We create density graph to check the distribution of n_steps. The graph below shows the distributions of minutes depending on if the average rating is missing or not. It shows, two distributions are similiar and have different mean. Thus, we can use absolute mean difference of minutes as our test statistics to test dependency of average rating by performing permutation test.

<iframe src="assets/Minutes_by_Missingness_of_Average_rating.html" width=800 height=600 frameBorder=0></iframe>

null hypothesis: The missingness of average rating does not depends on minutes.
alternative hypothesis: The missingness of average rating depends on minutes.

This is the graph created by permutation test. By comparing the observed statistic with the permutated data, we found the p_value is 0.83, so we failed to reject the null hypothesis. Also, by plotting the observed, we can get the same conclusion

<iframe src="assets/Empirical_distribution_of_the_Mean_differences_in_minutes_two.html" width=800 height=600 frameBorder=0></iframe>

We believe the missingness of the Average rating does depend on column n_steps. We repeated steps above to check the distribution of n_steps regarding of the missingness of average rating. The graph below is the distribution graphs. It shows, two distributions are similiar and have different mean. Thus, we can use absolute mean difference of n_steps as our test statistics to test dependency of average rating by performing permutation test.

<iframe src="assets/n_steps_by Missingness_of_Average_rating.html" width=800 height=600 frameBorder=0></iframe>

null hypothesis: The missingness of average rating does not depends on minutes.
alternative hypothesis: The missingness of average rating depends on minutes.

This is the graph created by permutation test. By comparing the observed statistic with the permutated data, we found the p_value is 0.022, so we rejected the null hypothesis. The missingness of average rating does depend on n_steps. Also, by plotting the observed, we can get the same conclusion

<iframe src="assets/Empirical_distribution_of_the_Mean_differences_in_n_steps.html" width=800 height=600 frameBorder=0></iframe>
