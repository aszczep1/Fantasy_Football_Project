# Fantasy_Football_Project

[Project link](./preprocessing&analysis/Fantasy_Football_Prediction_Project.ipynb)

This project aims to accurately predict fantasy points for NFL players based on data from 1999-2023. It looks to predict the points per player for the 2023 season as its testing data. To obtain accurate predictions, we built a linear regression model and several machine-learning models based on the 8,714 data points that were originally collected from, [Github](https://github.com/nflverse/nflverse-data/releases/tag/player_stats). Although this begins with more players, a cap was put on the yards a player needed to be included in the dataset. Orthogonal data was also collected to better predict fantasy scores at the beginning of an NFL season. Different datasets were created based on position. Preprocessing ended in a Quarterback dataset, a Wide Receiver/Tight End dataset, and a Runningback dataset. Each of these had a few different variables and had different success with the models. The quarterback dataset, only having 1,488 data points, had the most success with the XGBoost model, with an RMSE of 103.51 and the top three features being average fantasy points in the year prior, starter or backup at the beginning of the season, and severe injury before the season. The lack of predictability here can most likely be fixed with more data points, some feature transformation, and cross-validation. The wide receiver and tight end model had the most success overall. Surprisingly, its best model was the multiple regression model with an RMSE of 56.59 and an r-squared value of .562. Although, this dataset had the most consistent RMSE among all the models. This model may be improved through cross-validation and feature transformation. It is not a surprise that this dataset had the best predictability because it had the most data points with 4,842 data points. The running back data frame had more predictive power than the quarterback data, having 2,384 data points. However, it was still not very predictive. The best model here was also the multiple regression with an RMSE of 81.9 for the testing set. However, there was a RMSE of 62.73 for the training set. This indicates that our model was overfitting. This can be fixed by running a ridge or lasso regression on the dataset. The Gradient Boosting algorithm was not far behind with an RMSE of 82.03 and its most important features were average fantasy points the year prior, years in the league, and rushing epa. The problems with these models could be fixed by adding data points, fixing overfitting, and feature transformation. Overall, as shown in the bar graph visualizations of the top ten players for all the categories these predictions held some weight to them. The highest inaccuracies came from when a player got hurt during the season. 

### Data collection: 
#### Demographic information
 _Collected by: Alex Szczepanski_
This data sought to pull the height, weight, college, high school, high school state, place of birth, draft team, draft number/round, draft year, and birthday of every player in the dataset. 

The data was collected using Selenium and BeautifulSoup in Python. The code used for this can be found [here](./Scrape_Info/Clean_Scraping_File.ipynb). 

Source:  https://www.pro-football-reference.com/


#### Injury data
_Collected by: Maggie Conners, Brett Gaebel, Conor Hughes, Michael Leahey, Michael Sternbach, and  Alex Szczepanski_

This data collection sought to pull information on if the player was injured the prior season, injury type, injury severity, and if it was season-ending.

The data was collected by manually searching the internet and using various resources like Draft Sharks, Rotowire, Wikipedia, and other news outlets. 


#### Coaching data 
_Collected by: Conor Hughes_

This data sought to collect the coach's name, team, season, first year with the team or not, years coached, wins, losses, and ties. 

The data was manually collected from Wikipedia.

#### Starter Data (Quarterbacks)
_Collected by: Michael Leahey_

This binary variable sought to teach the algorithm if a quarterback was named a starter at the beginning of the season. This way first-year starters are recognized by the algorithms. 

This data was collected manually across different news sites.


### Variable creation, cleaning, and preprocessing
_Conducted by: Micheal Sternbach_ 
- Losing a high-value teammate 
- Binning height
- Binning weight 
- Binning draft round 
- Age
- Fantasy points next season (dependent variable)
- Tenure in the league 
- Making string variables consistent (for the get_dummies function)
- Check for nulls


### Data visualization
_Conducted by: Micheal Sternbach_

For each position group, there are three heat maps:
1. This heat map depicts the correlation between each of the variables. However, it only shows where the correlation is >=0.8. A lot of these variables will end up being dropped if they correlate with variables other than the dependent variable. This is to avoid redundancy. 
2. This heat map shows the correlation that the variables have with the dependent variable (fantasy points next season). Variables with high correlation should mean that they are important to the models. 
3. This heat map depicts the correlation of all the variables among themselves. Any of the variables here with a more yellow or reddish color should be observed in greater depth. 

For each position group, there is also a strip plot for the binned variables (height, weight, and draft pick): 

* Wide Receiver
1. Binned Weight - This graph has a fairly uniform distribution, with some individuals in the 180-199 pound bin outperforming everyone else. 
2. Binned Height -  This graph has a normal distribution with the top 72-74 inch receivers having the best average points 
3. Binned Draft Pick - This graph is fairly uniform with some high point scorers in every bin. However, it seems that if a player is drafted in the top 5, they have a higher average floor.

* Running back
1. Binned Weight - There are no running backs in the 160-179 bin and a small number in the 260-279 bin. The most populated bin here (200-219) also has the highest average fantasy points for the top players by almost 100 points. This indicates that it is the optimal weight for a running back. 
2. Binned Height - There are almost no runningbacks taller than 74 inches. The ones who seem to do okay. However, the most populated and highest-performing bin is the 69-71 inch bin. 
3. Binned Draft Pick - It seems that the earlier draft picks for running backs have a high floor especially if drafted before the late second round.
   
* Quarterback 
1. Binned Weight - Most quarterbacks are between 200 and 239 pounds. The 220-239 pound bin has the top performers on average. 
2. Binned Height -  Most quarterbacks are between 72 and 77 inches. There is no significant difference between the groups. 
3. Binned Draft Pick - The average points scored between the groups seem to be sparse. However, the top 5 pick bin is the most densely populated. But, the early first-round bin has the highest average point scorers. 

### Analysis and Visualization
_Conducted by: Alex Szczepanski_

For each position group, there were six different models run to try to find the lowest RMSE: 
- Multiple regression
-- Prediction of points year to year 
-- Prediction of points 2023
- Random Forest 
- XGBoost 
- Gradient Boosting 
- Voting Regressor
