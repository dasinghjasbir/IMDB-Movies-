# Supress Warnings

import warnings
warnings.filterwarnings('ignore')

# Import the numpy and pandas packages

import numpy as np
import pandas as pd

## Task 1: Reading and Inspection

-  ### Subtask 1.1: Import and read

Import and read the movie database. Store it in a variable called `movies`.

movies = pd.read_csv("D:\imdb.csv")# Write your code for importing the csv file here
movies

-  ### Subtask 1.2: Inspect the dataframe

Inspect the dataframe's columns, shapes, variable types etc.

movies.shape # Getting all the rows and columns

# Write your code for inspection here
movies.info() # Inspecting number of Columns and rows with all Dtypes and counting not null values

## Task 2: Cleaning the Data

-  ### Subtask 2.1: Inspect Null values

Find out the number of Null values in all the columns and rows. Also, find the percentage of Null values in each column. Round off the percentages upto two decimal places.

# Write your code for column-wise null count here
movies.isnull().sum().sort_values(ascending=False) #sort function is udes to examine and sort in descending order using False

# Write your code for row-wise null count here
movies.isnull().sum(1).sort_values(ascending = False)

# Write your code for column-wise null percentages here

movies.isnull().sum().sort_values(ascending = False)/len(movies)*100

-  ### Subtask 2.2: Drop unecessary columns

For this assignment, you will mostly be analyzing the movies with respect to the ratings, gross collection, popularity of movies, etc. So many of the columns in this dataframe are not required. So it is advised to drop the following columns.
-  color
-  director_facebook_likes
-  actor_1_facebook_likes
-  actor_2_facebook_likes
-  actor_3_facebook_likes
-  actor_2_name
-  cast_total_facebook_likes
-  actor_3_name
-  duration
-  facenumber_in_poster
-  content_rating
-  country
-  movie_imdb_link
-  aspect_ratio
-  plot_keywords

movies.columns # Checking all the columns before proceeding to the next step

# Write your code for dropping the columns here. It is advised to keep inspecting the dataframe after each set of operations 

movies_df = movies.drop([
      "color",
    "director_facebook_likes",
    "actor_1_facebook_likes",
    "actor_2_facebook_likes",
    "actor_3_facebook_likes",
    "actor_2_name",
    "cast_total_facebook_likes",
    "actor_3_name",
    "duration",
    "facenumber_in_poster",
    "content_rating",
    "country",
    "movie_imdb_link",
    "aspect_ratio",
    "plot_keywords"
    ], axis = 1)


    

movies_df.columns # Checking for new columns

-  ### Subtask 2.3: Drop unecessary rows using columns with high Null percentages

Now, on inspection you might notice that some columns have large percentage (greater than 5%) of Null values. Drop all the rows which have Null values for such columns.

# Write your code for dropping the rows here
round(movies_df.isnull().sum().sort_values(ascending=False)/len(movies_df)*100, 2)

movies_df = movies_df[movies_df['gross'].notnull()]
movies_df = movies_df[movies_df['budget'].notnull()]
round(movies_df.isnull().sum().sort_values(ascending=True)/len(movies_df)*100,2)

round(movies_df.isnull().sum().sort_values(ascending=False)/len(movies_df)*100, 2)

-  ### Subtask 2.4: Fill NaN values

You might notice that the `language` column has some NaN values. Here, on inspection, you will see that it is safe to replace all the missing values with `'English'`.

# Write your code for filling the NaN values in the 'language' column here
(movies_df.isnull().sum(axis = 1).sort_values(ascending=False) > 5).sum()

movies_df = movies_df[movies_df.isnull().sum(1).sort_values(ascending=False) <= 5]
movies_df

round(movies_df.isnull().sum(0).sort_values(ascending=False)/len(movies_df)*100, 2)

movies_df.groupby('language').language.count().sort_values(ascending=False)

movies_df.language.describe()

movies_df.language = movies_df.language.fillna('English')


movies_df


round(movies_df.isnull().sum(0).sort_values(ascending=False)/len(movies_df)*100, 2)
# Assessing the dataframe to make sure language column is 0.00

-  ### Subtask 2.5: Check the number of retained rows

You might notice that two of the columns viz. `num_critic_for_reviews` and `actor_1_name` have small percentages of NaN values left. You can let these columns as it is for now. Check the number and percentage of the rows retained after completing all the tasks above.

# Write your code for checking number of retained rows here
round(len(movies_df)/len(movies) * 100,2) 
#dividing the new length of the movies dataframe over original dataframe and multiplying by 100 then using round 
# function to 2 decimal places

**Checkpoint 1:** You might have noticed that we still have around `77%` of the rows!

## Task 3: Data Analysis

-  ### Subtask 3.1: Change the unit of columns

Convert the unit of the `budget` and `gross` columns from `$` to `million $`.

# Write your code for unit conversion here
movies_df['budget'] = movies_df['budget'].apply(lambda x: x/1000000) # or movies['budget']/1000000
movies_df['gross']  = movies_df['gross'].apply(lambda x: x/1000000) # or movies['gross']/1000000


movies_df

-  ### Subtask 3.2: Find the movies with highest profit

    1. Create a new column called `profit` which contains the difference of the two columns: `gross` and `budget`.
    2. Sort the dataframe using the `profit` column as reference.
    3. Plot `profit` (y-axis) vs `budget` (x- axis) and observe the outliers using the appropriate chart type.
    4. Extract the top ten profiting movies in descending order and store them in a new dataframe - `top10`

# Write your code for creating the profit column here
movies_df['profit'] = movies_df['gross'] - movies_df['budget']

movies_df #Inspecting whether Column profit has been added to the dataframe or not

# Write your code for sorting the dataframe here
movies_df.sort_values(by = 'profit', ascending=False)

# Write code for profit vs budget plot here
import matplotlib.pyplot as plt

diagram = plt.hist(movies_df['gross'], histtype = 'bar') 
diagram = plt.xlabel("budget")
diagram = plt.ylabel("profit")
plt.show()

# Write your code to get the top 10 profiting movies here
top10 =  movies_df.sort_values(by = 'profit', ascending=False).head(10) 

top10

-  ### Subtask 3.3: Drop duplicate values

After you found out the top 10 profiting movies, you might have noticed a duplicate value. So, it seems like the dataframe has duplicate values as well. Drop the duplicate values from the dataframe and repeat `Subtask 3.2`. Note that the same `movie_title` can be there in different languages. 

# Write your code for dropping duplicate values here
movies_df = movies_df.drop_duplicates(keep = 'first')

# Write code for repeating subtask 2 here
top10 =  movies_df.sort_values(by = 'profit', ascending=False).head(10) 
top10

**Checkpoint 2:** You might spot two movies directed by `James Cameron` in the list.

 ###   Subtask 3.4: Find IMDb Top 250
    1. Create a new dataframe `IMDb_Top_250` and store the top 250 movies with the highest IMDb Rating (corresponding to the column: `imdb_score`). Also make sure that for all of these movies, the `num_voted_users` is greater than 25,000.Also add a `Rank` column containing the values 1 to 250 indicating the ranks of the corresponding films.
   2. Extract all the movies in the `IMDb_Top_250` dataframe which are not in the English language and store them in a new dataframe named `Top_Foreign_Lang_Film`.

# Write your code for extracting the top 250 movies as per the IMDb score here. Make sure that you store it in a new dataframe 
# and name that dataframe as 'IMDb_Top_250'

IMDb_Top_250 = movies_df[movies_df['num_voted_users'] > 25000].sort_values(by = 'imdb_score', ascending = False).head(250)
IMDb_Top_250

IMDb_Top_250['Rank'] = IMDb_Top_250['imdb_score'].rank(method = 'first', ascending = False)
IMDb_Top_250 

# Write your code to extract top foreign language films from 'IMDb_Top_250' here

# Top_Foreign_Lang_Film = 

IMDb_Top_250[IMDb_Top_250['language'] != 'English']

**Checkpoint 3:** Can you spot `Veer-Zaara` in the dataframe?

- ### Subtask 3.5: Find the best directors

    1. Group the dataframe using the `director_name` column.
    2. Find out the top 10 directors for whom the mean of `imdb_score` is the highest and store them in a new dataframe `top10director`.  Incase of a tie in IMDb score between two directors, sort them alphabetically. 

# Write your code for extracting the top 10 directors here

top10director = movies_df.groupby('director_name').imdb_score.mean().sort_values(ascending=False).head(10)
top10director

**Checkpoint 4:** No surprises that `Damien Chazelle` (director of Whiplash and La La Land) is in this list.

-  ### Subtask 3.6: Find popular genres

You might have noticed the `genres` column in the dataframe with all the genres of the movies seperated by a pipe (`|`). Out of all the movie genres, the first two are most significant for any film.

1. Extract the first two genres from the `genres` column and store them in two new columns: `genre_1` and `genre_2`. Some of the movies might have only one genre. In such cases, extract the single genre into both the columns, i.e. for such movies the `genre_2` will be the same as `genre_1`.
2. Group the dataframe using `genre_1` as the primary column and `genre_2` as the secondary column.
3. Find out the 5 most popular combo of genres by finding the mean of the gross values using the `gross` column and store them in a new dataframe named `PopGenre`.

# Write your code for extracting the first two genres of each movie here
t_genre = movies_df.genres.str.split('|', expand = True).iloc[:, 0:2]
t_genre.columns = ['genre_1', 'genre_2']
t_genre.genre_2.fillna(t_genre.genre_1, inplace=True)
t_genre

movies_df = pd.concat([movies_df,t_genre], axis = 1)
movies_df

# Due to my mistake I created duplicates and then I found this function online and used it to delete all the duplicates 
                               
movies_df = movies_df.loc[:, ~movies_df.columns.duplicated()]
movies_df


 # Write your code for grouping the dataframe here

movies_by_segment = movies_df.groupby(['genre_1', 'genre_2']).gross.mean().sort_values(ascending=False)
movies_by_segment

 # Write your code for getting the 5 most popular combo of genres here
PopGenre = movies_by_segment = movies_df.groupby(['genre_1', 'genre_2']).gross.mean().sort_values(ascending=False).head(5)
PopGenre


**Checkpoint 5:** Well, as it turns out. `Family + Sci-Fi` is the most popular combo of genres out there!

###  Subtask 3.7: Find the critic-favorite and audience-favorite actors

    1. Create three new dataframes namely, `Meryl_Streep`, `Leo_Caprio`, and `Brad_Pitt` which contain the movies in which the actors: 'Meryl Streep', 'Leonardo DiCaprio', and 'Brad Pitt' are the lead actors. Use only the `actor_1_name` column for extraction. Also, make sure that you use the names 'Meryl Streep', 'Leonardo DiCaprio', and 'Brad Pitt' for the said extraction.
    2. Append the rows of all these dataframes and store them in a new dataframe named `Combined`.
    3. Group the combined dataframe using the `actor_1_name` column.
    4. Find the mean of the `num_critic_for_reviews` and `num_users_for_review` and identify the actors which have the highest mean.
    5. Observe the change in number of voted users over decades using a bar chart. Create a column called `decade` which represents the decade to which every movie belongs to. For example, the  `title_year`  year 1923, 1925 should be stored as 1920s. Sort the dataframe based on the column `decade`, group it by `decade` and find the sum of users voted in each decade. Store this in a new data frame called `df_by_decade`.

# Write your code for creating three new dataframes here
Brad_Pitt = pd.DataFrame(movies_df.loc[movies_df.actor_1_name=='Brad Pitt'])
Brad_Pitt.head()
# Meryl_Streep = movies_df[movies_df['actor_1_name'] == ' Meryl Streep']

# Leo_Caprio = pd.DataFrame(movies_df.loc[movies_df.actor_1_name == 'Leo_Caprio'])
Leo_Caprio = pd.DataFrame(movies_df.loc[movies_df.actor_1_name=='Leonardo DiCaprio'])
Leo_Caprio.head()

Meryl_Streep = pd.DataFrame(movies_df.loc[movies_df.actor_1_name=='Meryl Streep'])
Meryl_Streep.head()


# Write your code for combining the three dataframes here
combined = pd.concat([Meryl_Streep,Leo_Caprio,Brad_Pitt],axis=0)
combined

# Write your code for grouping the combined dataframe here
combined.groupby(by = 'actor_1_name').head()


# Write the code for finding the mean of critic reviews and audience reviews here
combined.groupby('actor_1_name').num_critic_for_reviews.mean()

combined.num_user_for_reviews.astype('int')

combined.groupby('actor_1_name').num_user_for_reviews.mean().sort_values(ascending=False)

combined.groupby( 'actor_1_name')[['num_critic_for_reviews','num_user_for_reviews']].mean()

**Checkpoint 6:** `Leonardo` has aced both the lists!
