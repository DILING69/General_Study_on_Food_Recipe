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
3. We check the unique for calories, and drop any rows with colories more than 15k. 15k calories and above from a recipe is not a reasonable, since the recommended daily calorie intake is 2000-2500 calorie

Below is the first five rows of the eda_df

<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th></th>\n      <th>name</th>\n      <th>id</th>\n      <th>minutes</th>\n      <th>contributor_id</th>\n      <th>submitted</th>\n      <th>tags</th>\n      <th>nutrition</th>\n      <th>n_steps</th>\n      <th>steps</th>\n      <th>description</th>\n      <th>ingredients</th>\n      <th>n_ingredients</th>\n      <th>average_rating</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>1 brownies in the world    best ever</td>\n      <td>333281</td>\n      <td>40</td>\n      <td>985201</td>\n      <td>2008-10-27</td>\n      <td>[\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'for-large-groups\', \'desserts\', \'lunch\', \'snacks\', \'cookies-and-brownies\', \'chocolate\', \'bar-cookies\', \'brownies\', \'number-of-servings\']</td>\n      <td>[138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]</td>\n      <td>10</td>\n      <td>[\'heat the oven to 350f and arrange the rack in the middle\', \'line an 8-by-8-inch glass baking dish with aluminum foil\', \'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted\', \'remove from heat and let cool to room temperature\', \'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated\', \'add cooled chocolate and mix until uniform in color\', \'add flour and stir until just incorporated\', \'transfer batter to the prepared baking dish\', \'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes\', \'remove from the oven and cool completely before cutting\']</td>\n      <td>these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you\'ll ever make.....sereiously! there\'s no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they\'re pure heaven!</td>\n      <td>[\'bittersweet chocolate\', \'unsalted butter\', \'eggs\', \'granulated sugar\', \'unsweetened cocoa powder\', \'vanilla extract\', \'brewed espresso\', \'kosher salt\', \'all-purpose flour\']</td>\n      <td>9</td>\n      <td>4.0</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>1 in canada chocolate chip cookies</td>\n      <td>453467</td>\n      <td>45</td>\n      <td>1848091</td>\n      <td>2011-04-11</td>\n      <td>[\'60-minutes-or-less\', \'time-to-make\', \'cuisine\', \'preparation\', \'north-american\', \'for-large-groups\', \'canadian\', \'british-columbian\', \'number-of-servings\']</td>\n      <td>[595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]</td>\n      <td>12</td>\n      <td>[\'pre-heat oven the 350 degrees f\', \'in a mixing bowl , sift together the flours and baking powder\', \'set aside\', \'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy\', \'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined\', \'add in the flour mixture to the wet ingredients and blend until combined\', \'scrape down the sides of the bowl and add the chocolate chips\', \'mix until combined\', \'scrape down the sides to the bowl again\', \'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking\', \'bake for 10 - 15 minutes or until golden brown on the outside and soft &amp; chewy in the center\', \'serve hot and enjoy !\']</td>\n      <td>this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don\'t have margarine or don\'t like it, then just use butter (softened) instead.</td>\n      <td>[\'white sugar\', \'brown sugar\', \'salt\', \'margarine\', \'eggs\', \'vanilla\', \'water\', \'all-purpose flour\', \'whole wheat flour\', \'baking soda\', \'chocolate chips\']</td>\n      <td>11</td>\n      <td>5.0</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>412 broccoli casserole</td>\n      <td>306168</td>\n      <td>40</td>\n      <td>50969</td>\n      <td>2008-05-30</td>\n      <td>[\'60-minutes-or-less\', \'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'side-dishes\', \'vegetables\', \'easy\', \'beginner-cook\', \'broccoli\']</td>\n      <td>[194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]</td>\n      <td>6</td>\n      <td>[\'preheat oven to 350 degrees\', \'spray a 2 quart baking dish with cooking spray , set aside\', \'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce\', \'pour into baking dish , sprinkle remaining cheese over top\', \'bake for 25 minutes or until cheese is lightly browned\', \'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes\']</td>\n      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>\n      <td>[\'frozen broccoli cuts\', \'cream of chicken soup\', \'sharp cheddar cheese\', \'garlic powder\', \'ground black pepper\', \'salt\', \'milk\', \'soy sauce\', \'french-fried onions\']</td>\n      <td>9</td>\n      <td>5.0</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>millionaire pound cake</td>\n      <td>286009</td>\n      <td>120</td>\n      <td>461724</td>\n      <td>2008-02-12</td>\n      <td>[\'time-to-make\', \'course\', \'cuisine\', \'preparation\', \'occasion\', \'north-american\', \'desserts\', \'american\', \'southern-united-states\', \'dinner-party\', \'holiday-event\', \'cakes\', \'dietary\', \'christmas\', \'thanksgiving\', \'low-sodium\', \'low-in-something\', \'taste-mood\', \'sweet\', \'4-hours-or-less\']</td>\n      <td>[878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0]</td>\n      <td>7</td>\n      <td>[\'freheat the oven to 300 degrees\', \'grease a 10-inch tube pan with butter , dust the bottom and sides with flour , and set aside\', \'in a large mixing bowl , cream the butter and sugar with an electric mixer and add the eggs one at a time , beating after each addition\', \'alternately add the flour and milk , stirring till the batter is smooth\', \'add the two extracts and stir till well blended\', \'scrape the batter into the prepared pan and bake till a cake tester or knife blade inserted in the center comes out clean , about 1 1 / 2 hours\', \'cool the cake in the pan on a rack for 5 minutes , then turn it out on the rack to cool completely\']</td>\n      <td>why a millionaire pound cake?  because it\'s super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.</td>\n      <td>[\'butter\', \'sugar\', \'eggs\', \'all-purpose flour\', \'whole milk\', \'pure vanilla extract\', \'almond extract\']</td>\n      <td>7</td>\n      <td>5.0</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>2000 meatloaf</td>\n      <td>475785</td>\n      <td>90</td>\n      <td>2202916</td>\n      <td>2012-03-06</td>\n      <td>[\'time-to-make\', \'course\', \'main-ingredient\', \'preparation\', \'main-dish\', \'potatoes\', \'vegetables\', \'4-hours-or-less\', \'meatloaf\', \'simply-potatoes2\']</td>\n      <td>[267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]</td>\n      <td>17</td>\n      <td>[\'pan fry bacon , and set aside on a paper towel to absorb excess grease\', \'mince yellow onion , red bell pepper , and add to your mixing bowl\', \'chop garlic and set aside\', \'put 1tbsp olive oil into a saut pan , along with chopped garlic , teaspoons white pepper and a pinch of kosher salt\', \'bring to a medium heat to sweat your garlic\', \'preheat oven to 350f\', \'coarsely chop your baby spinach add to your heated pan , stir frequently for approximately 5 min to wilt\', \'add your spinach to the mixing bowl\', \'chop your now cooled bacon , and add it to the mixing bowl\', \'add your meatloaf mix to the bowl , with one egg and mix till thoroughly combined\', \'add your goat cheese , one egg , 1 / 8 tsp white pepper and 1 / 8 tsp of kosher salt and mix till thoroughly combined\', \'transfer to a 9x5 meatloaf pan , and cook for 60 min or until the internal temperature is at least 160f\', \'let stand for 5min\', \'melt 1tbsp unsalted butter into a frying pan , and cook up to three eggs at a time\', \'crack each egg into a separate dish , in order to prevent egg shells from reaching the pan , then add salt and pepper to taste\', \'wait until the egg whites are firm looking , but slightly runny on top before flipping your eggs\', \'after flipping , wait 10~20 seconds before removing each egg and placing it over your slices of meatloaf\']</td>\n      <td>ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.</td>\n      <td>[\'meatloaf mixture\', \'unsmoked bacon\', \'goat cheese\', \'unsalted butter\', \'eggs\', \'baby spinach\', \'yellow onion\', \'red bell pepper\', \'simply potatoes shredded hash browns\', \'fresh garlic\', \'kosher salt\', \'white pepper\', \'olive oil\']</td>\n      <td>13</td>\n      <td>5.0</td>\n    </tr>\n  </tbody>\n</table>

This is the visualization for calories distribution. As the calories increases for dishes, the number of dishes decreases dramatically.

<iframe src="assets/distribution_of_calories_less_than_15K.html" width=800 height=600 frameBorder=0></iframe>
