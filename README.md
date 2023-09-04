# NBA Career Prediction
------------------------
# Table of Contents
- [Executive Summary](#first-point)
- [Introduction](#second-point)
- [Data Cleaning](#third-point)
- [Initial Exploratory Data Analysis (EDA)](#fourth-point)
- [Pre-Processing and Modeling](#fifth-point)
- [Conclusion](#sixth-point)
- [References](#seventh-point)
------------------------
## Executive Summary<a name="first-point"></a>

This model will attempt to predict whether or not a player in the National Basketball Association (NBA) will have made an All-NBA team. Ultimately, we want to know what in-game and advanced statistics should a team focus on when looking for a player to add to their team. While a player can be first team, second team or third team All-NBA, this model simply seeks to predict if a player made any of them in their career.
- The 24,000 row dataset was downloaded off of Kaggle (reference below) that has information for each player in the NBA from 1950 to 2017.
- There were 3 datasets given that included information about each player and his statistics.

## Introduction<a name="second-point"></a>

This model attempts to predict whether or not a player in the National Basketball Association (NBA) will has a statline that predicts if he made an All-NBA team. The goal is to find the features that most contribute toward a player's success so that teams can know which metrics to value.

Every team, in one way or another, tries to predict the performance of player, particularly for players that are young or said to have a lot of potential. The goal would be to add a player to a team that will help them win now, but especially in the future. If that can be predicted, it will allow a team to do what every team sets out to do each year: win a championship.

Front office staff, scouts and coaches are the ones that experience this problem the most (we’ll sum all of them up by referring to them as “the team”). The team would benefit from this as they seek to add players every year that would best help them win a championship. The difficulty in figuring out if a player will be good is very complex. Not only is it statistically complex but there are many external factors that make an impact on a player’s career. Examples include family life, unforseeable events, injuries, and much more. Therefore, since we can’t predict every factor that could determine a player’s success in the NBA, it’s worth trying to harness the more concrete, statistical side. While we will never be able to create a model that perfectly predicts such an outcome, the goal is to do better than random selection in our prediction of an NBA player's career outcome.

As explained above, categorical prediction will be method of choice. 

## Data Cleaning<a name="third-point"></a>

Three datasets were downloaded to create the final, clean dataset used in analysis. 

Two of them were information about each player. This includes name, height, weight, college, year born, birth city, birth state, year career started, year career ended, position, height, and birthday. Between these two datasets, there was quite a bit of cleaning that needed to be done in order to prepare for merging with the third dataset, which I will get to in a little bit. One major clean up was the name column, because in one of the datasets, there were some names with astrisks at the end indicating that player was inducted into the basketball Hall of Fame. So a new, binary column was created indicating a player's Hall of Fame status and I updated the list of Hall of Fame players to include those inducted in the last 5 years (since the dataset was made). Another major clean up had to do with duplicate names. There were 50 players who had duplicate names, whether it be from their father playing in the NBA years before them or just by chance. Since we could not find a clean way to distinguish between the seperate persons, we removed those names as they could have a significant impact on the outcome.

After the merged dataset was cleaned up, I then merged the cleaned dataset with the season stats dataset. The season stats includes every player from every year that was in the NBA from 1950 to 2017. The season stats dataset includes 53 columns of information and statistics ranging from the common numbers like points, assists and rebounds to deeper statistics like player efficiency rating (PER), box plus/minus (BPM), and more. After lots of investigation, it became clear there was a noticeable difference in the statistics tracked before and after 1974. Many statistics weren't kept before then. If I had kept all rows, it would have resulted in deleting almost all of the statistics that weren't kept before 1974. Therefore, I excluded all rows before 1974 so we could keep the statistics.

Third, I converted all non-numeric columns to binary/dummy columns. For example, the college column was converted into dummy columns. This one specifically added about 300 columns. This was simply to see if any particular college correlated with hall of fame status. If not, they will be removed. The only non-numeric column left alone was name.

Finally, I had to add the target column indicating whether a player had made an All-NBA team. I was able to find a table online, copied its contents into my notebook, and added them as a column to my dataframe (this occurred in my Preprocessing Notebook). I then changed the column to binary with 1 indicating a player made at least one All-NBA team in their career.

After cleaning, I had about 20,500 rows of a player's statistics in a given year.

I debated adding things like awards and championships. However, after some thinking, I decided not to. It may seem like the wrong choice since awards and championships certainly contribute to HOF status, there is no debate there. Great players win championships, end of story. However, my goal is to predict what metrics make players great. Awards and championships are biproducts of other metrics like points, blocks, assists, etc. They're even biproducts of advanced metrics like win shares (WS), value over replacement player (VORP), etc. Therefore, in order to help teams find/create good players, it would not be helpful to tell them to make a player better by winning championships or awards, that's obvious. Teams want to know what to focus on in order to get there. Therefore, you will not see awards and championships considered here.

## Initial Exploratory Data Analysis (EDA)<a name="fourth-point"></a>

EDA was done in Jupyter notebooks and Tableau. The notebook shows correlations, particularly with the `all_nba_selections` column. Tableau simply is showing relationships between variables.

First, EDA showed trends in points scored, shot percentages, rebounds, assists, and more over time. As rules, strategies and technology change, these statistics show that change. Points scored has increased significantly in recent years along with fouls. There is a near record low in minues played. 3-point shot attempts have increased significantly, particularly in the last 10 years being that it's doubled over that time. Some things haven't changed much. Average height and weight of players has stayed the same and field goal percentage has stayed the same.

Second, hall of fame status is the only binary indicator we initially had. So I ran multiple correlation heatmaps to see which variables correlated best with it. Since it's a binary column, there was "great" correlation numbers. However, the highest were points, win shares, and a tie between rebounds, minutes played, and turnovers. Assists were up there as well.

Finally, initial EDA showed that choice of college didn't make a huge difference. The schools with the most number of Hall of Fame players are University of North Carolina and UCLA with 6 players each. Those two schools are historically good at basketball with two historic coaches having coached there in the 60s to the 80s. Outside of that, there were not many schools with multiple Hall of Fame players. Further, I created dummy columns for each college listed (over 370 in total) and none of them had a significant correlation with hall of fame status. Therefore, I do not see any evidence indicating college has an impact.

## Pre-Processing and Modeling<a name="fifth-point"></a>

Before pre-processing, I created a seperate dataframe that removed players whose `year_end` was greater than 2012. This was to remove players who potentially could be current players who were not eligible for the HOF when this dataset was made. After doing that, I found that about 3.2% of players were inducted into the Hall of Fame, therefore, 96.8% of players were not. This means my model needs to have an accuracy score better than 96.8% to show that the model can do better than chance.

To get a good idea of the features that most impact Hall of Fame status, I used SelectKBest to get the top 30 features. The following were in the top 10:
1. VORP
2. WS
3. FTA
4. FT
5. OWS
6. PTS
7. FG
8. 2P
9. FGA
10. 2PA

It's interesting to see simple categories such as points and free throws in here while more mathematically complex numbers like VORP and WS in here as well. It's worth noting that VORP is by far the highest with WS easily coming in second as well.

As a quick side-note, VORP stands for Value Over Replacement Player. This is a box score estimate of the points per 100 team possessions that a player contributed above a replacement-level player (-2.0). WS stands for Win Shares. This is an estimate of the number of wins contributed by a player. For both, the higher, the better.

While 3-point shots are increasing and are relatively new to the game (at least in the volume it's currently at), it's worth noting that right now, there are only two features in the top 30 that are related to 3-point shots, but they are at the very bottom (28 and 29). I believe this will change over time, but currently it doesn't make a major impact. For "basic" numbers, points are the theme. Players that score more points have a better chance of making an All-NBA team. The next "basic" numbers that matter are rebounds and assists, in that order.

I did a pipeline and grid search comparing the following:
- Scalers: Standard and MinMax
- Feature Engineering: PCA and SelectKBest
- Models: LogisticRegression, DecisionTreeClassifier, and SVC
- Hyperparameters: n_components (PCA), k features (SelectKBest), C-value (Logistic and SVC), kernel (SVC), and max_depth (DecisionTree)

After running this through a grid search and doing further model comparisons, the best performing value was Logistic Regression, C=1, SelectKBest, k=7, Standard Scaler. When running this model, it gave a test score of 0.978. Since 94.2% of players don't make an All-NBA team, a 97.8% score is good and better than chance.

When evaluating recall and precision, I believe precision is the more valuable of the two. This is because I don't want to have too many false positives, as the model would overvalue players who are not Hall of Fame caliber. Since the goal is to show teams the things that matter in a player's on-court success, I want to make sure precision is high rather than recall.

My precision score was 0.86 and recall was 0.67. Below illustrates the trade-off between the two based on the threshold in predictions:

![image](https://github.com/aiden-bailey/Capstone/assets/127895132/92ce80e6-c2cb-49ba-b65b-810153fd1346)

As you can see, recall never reaches a high value without severe decreases in precision. Therefore, I chose to leave the threshold at 0.50.

It's worth noting that these stats are on a per-game basis. This helped the model perform slightly better and makes the findings more applicable.

## Conclusion<a name="sixth-point"></a>

Overall, my model performed well but certainly has limitations:
- More recent players may not have had time to develop their skills long enough to make an All-NBA team.
- The top 30 best features include things like `minutes_played` and `games_started`. These things are likely a result of other features doing well like points scored. Players get more minutes in a game when they score more points in the minutes they do play. Therefore, it's somewhat circular reasoning. It's worth noting that as we evluate the features that are predictors of great players.
- Players that made only 1 All-NBA teams in their career may have had an outlier of a year for them. So this can mess with the performance of the model quite drastically.

For teams looking for in-game numbers to go by in choosing players to add to their team, points scored are the highest predictors of All-NBA status. After that it's rebounds, assists, steals, blocks, then fouls. So I'd recommend teams find players that emmulate this type of play.

Overall, our model does what we need it to do. Its accuracy of 97.8% is better than chance. The precision is high at the expense of recall.

We will miss some players in our predictions, but we can be confident that those we predict as hall of fame caliber are almost certainly that. Therefore, we can examine what makes these players successful and translate that into recruiting and building up players to be successful with their teams.

## References<a name="seventh-point"></a>

- Kaggle data reference: https://www.kaggle.com/code/piyush1912/nba-top-players-deep-learning/notebook
