# NBA Career Prediction
------------------------
## Executive Summary

This model attempts to predict whether or not a player in the National Basketball Association (NBA) will be a Hall of Fame player, good, average, or bad.
- The 24,000 row dataset was downloaded off of Kaggle (reference below) that has information for each player in the NBA from 1950 to 2017.

## Introduction

This model attempts to predict whether or not a player in the National Basketball Association (NBA) will be a Hall of Fame player, good, average, or bad.

Every team, in one way or another, tries to predict the performance of player, particularly for players that are young or said to have a lot of potential. The goal would be to add a player to a team that will help them win now, but especially in the future. If that can be predicted, it will allow a team to do what every team sets out to do each year: win a championship.

Front office staff, scouts and coaches are the ones that experience this problem the most (we’ll sum all of them up by referring to them as “the team”). The team would benefit from this as they seek to add players every year that would best help them win a championship. The difficulty in figuring out if a player will be good is very complex. Not only is it statistically complex but there are many external factors that make an impact on a player’s career. Examples include family life, unforseeable events, injuries, and much more. Therefore, since we can’t predict every factor that could determine a player’s success in the NBA, it’s worth trying to harness the more concrete, statistical side. While we will never be able to create a model that perfectly predicts such an outcome, the goal is to do better than random selection in our prediction of an NBA player's career outcome.

As explained above, categorical prediction will be method of choice. 

## Data Cleaning

Three datasets were downloaded to create the final, clean dataset used in analysis. 

Two of them were information about each player. This includes name, height, weight, college, year born, birth city, birth state, year career started, year career ended, position, height, and birthday. Between these two datasets, there was quite a bit of cleaning that needed to be done in order to prepare for merging with the third dataset, which I will get to in a little bit. One major clean up was the name column, because in one of the datasets, there were some names with astrisks at the end indicating that player was inducted into the basketball Hall of Fame. So a new, binary column was created indicating a player's Hall of Fame status and I updated the list of Hall of Fame players to include those inducted in the last 5 years (since the dataset was made). Another major clean up had to do with duplicate names. There were 50 players who had duplicate names, whether it be from their father playing in the NBA years before them or just by chance. Since we could not find a clean way to distinguish between the seperate persons, we removed those names as they could have a significant impact on the outcome.

After the merged dataset was cleaned up, I then merged the cleaned dataset with the season stats dataset. The season stats includes every player from every year that was in the NBA from 1950 to 2017. The season stats dataset includes 53 columns of information and statistics ranging from the common numbers like points, assists and rebounds to deeper statistics like player efficiency rating (PER), box plus/minus (BPM), and more. After lots of investigation, it became clear there was a noticeable difference in the statistics tracked before and after 1974. Many statistics weren't kept before then. If we kept all rows, it would have resulted in deleting almost all of the statistics that weren't kept before 1974. Therefore, we excluded all rows before 1974 so we could keep the statistics.

After cleaning, we had about 20,500 rows of a player's statistics in a given year.

## Exploratory Data Analysis

## Contents/Todo

- [X] Data Exploration
- [X] Data Cleaning and Preprocessing
- [ ] Exploratory Data Analysis
- [ ] Modelling
- [ ] Advanced Modelling

## References

- 
