<div align="center">

# 📈 Predicting NHL Goal Scoring 🏒

</div>

## 🎯 Project Overview

Having grown up in Canada, I've been watching hockey, specifically the National Hockey League, for as long as I can remember.  Over the last couple of years, two of the more frequent topics of conversation around the NHL have been the growth of advanced analytics (and their integration into traditional hockey operations departments), and the emergence of regulated sports gambling in the USA and Canada.  From a fan perspective, the timing of these trends has given rise to a unique moment in league history:  never before has the publicly available data been this advanced or accessible, and never before has the financial incentive been so high to have that privileged access.  

As a longtime fan of the league, and a relative newcomer to the study of data, I wanted to leverage my domain knowledge of the space to answer a question that I know is of considerable interest to hockey fans, fantasy league managers, and sports bettors alike: <br>
***Can a Machine Learning model (or models) be trained to accurately predict a player’s goal output for a new season, based on historical player statistics?***

The goal of this project is to train multiple regression models and compare their performance to find the model and hyperparameter mix that can most accurately predict how many goals a given player will score in their next seaon of play.  All scoring predictions will be prorated to a standard 82 game season, and a margin of error of +/-3 goals will be deemed acceptable for a 'correct' prediction.  The model deemed most accurate will then be scored on its ability to correctly predict winning bets based on real odds published by DraftKings Sportsbook (from the 2022-2023 season).  


## 📊 Dataset

The dataset was manually pulled from the NHL's public API, with each entry holding a player's personal information, and their gameplay statistics for one season of play.  The players of interest for this analysis were full-time NHL players who contributed to NHL goal scoring; filters were written into the pull requests to exclude players who did not meet the following criteria:
  - A player had to be of the position type 'forward' or 'defenseman' - goaltenders were excluded
  - A player had to have played at least 180 games AND with appearances in at least 3 NHL seasons

While the NHL's API is public, they do not maintain resources about how to access it, or what endpoints are explicitly avaiable.  Thankfully, there are many intrepid fans who have collected and documented this information for others to use.  From research on several blogs and GitHub repositories I identified that the data I was interested in came from two specific endpoints: `people` and `stats`.  The `people` end point contains biographical data such as a player's name, birth date, nationality, height, and weight.  The `stats` endpoint is where year-by-year gameplay statistics were accessible from.  A list of the player ID numbers as assigned by the NHL was also found, and was used to set up a looping pull request that would pull data for each valid player ID.  

The collected dataset had ~38,000 entries.  Some simple checks were ran to confirm that the player data was collected as desired.  Some of the checks included:
  - comparing the statistics for random players against their statistics on the NHL website, and on the third-party website hockeydb.com
  - comparing the goal totals of the 15 highest scoring players of all time against their totals in the record books

The cleaned dataset that was passed to the modeling phase of the project featured 13,503 entries and 41 features.

*NOTE: Since collecting the data for this project, the league has completely revamped its API as part of the rollout of its new statistics platform called NHL Edge.  As a result, the code for my pull requests is no longer valid.*


## 🚀 Project Workflow

### 🐛 [API Debugging]
Working with API calls was a new experience for me; writing the code needed to collect requisite data was a process that involved calling two different endpoints from the NHL API, setting up conditional statements to execute preliminary feature processing (passing on entries that did not satisfy particular requirements), and debugging error responses (where the response received differed from what was expected).  

*See the API_Debugging.ipynb notebook for details.*


### 💾 [Data Collection]
The NHL assigns playerd ID numbers to all players that play in the league.  After reviewing a list of the player IDs (which are well documented online), I set up batches of API calls that ran through lists of 5,000 numbers at a time, pulling the statistics for any player meeting my criteria for collection.  The collected data was then concatenated into a single DataFrame, holding just under 38,000 entries.  In total, the execution of the API pull requests took roughly 9 hours.  

*See the Data_Collect.ipynb notebook for details.*


### 🧹 [Data Cleaning & Prepocessing]
Below is a high-level overview of the data cleaning & preprocessing steps that were taken:
- Evaluated accuracy and completeness of dataset.  Performed confirmatory checks such as validating that the goal totals of the league's top all-time goal scorers were all correct.
- Removed all data prior to the 1979-1980 season, which is when the NHL merged with the other professional hockey league in North America, the WHA.  This merger consolidated all of the top talent into one league.
- For some features, the NHL only started tracking the data after the 1997-1998 season; missing values were populated with averages from the remainder of the dataset.  Averages were calculated separately for each of the four player positions in the dataset.
- Flattened the data such that only one entry exists for each player and season combination.  When the data was collected, players would have multiple entries for a single season if they were traded to a different team in the middle of the season.
- Made team names consistent.  Some franchises have relocated or rebranded the years, resulting in individual franchises having multiple team names represented in the dataset.  After cleaning, each franchise is represented by their current team identity, only.
- Converted object datatypes to numerical.  Features like player height and time on ice were stored as objects and required REGEX string manipulation to convert them to numerical features that could be fed into the models.

*See the Cleaning_&_Preprocessing.ipynb notebook for details.*


### ⚙️ [Feature Engineering]
Below is a high-level overview of the feature engineering steps that were taken:
- Created a feature to track how many NHL seasons a player has appeared in, prior to and including the current entry
- Created a feature to indicate if a player was traded in the offseason prior to the current entry
- Prorated gameplay statistics to a full season 82-game pace
- For each gameplay statistic, created a second feature that contained an exponentially-weighted 3-year moving average of that statistic, for that player
- Created a shooting percentage feature, as well as a cumulative moving average of that statistic, for that player
- Set up the target feature, which is a player's prorated goal total for their n+1 season, where n is the season of the currenty entry
- Encoded categorical data using binary and one-hot encoding, as applicable.

*See the Features_Engineering.ipynb notebook for details.*


### 🔮 [Modeling]
The cleaned and processed data was used to train four different regression models:
- Linear Regression
- Support Vector Regression
- K-Nearest Neighbors Regression
- Gradient Boosting Regression
Each model was scored on three metrics:  the R2 score, the Mean Squared Error, and a novel prediction accuracy metric which assessed if the prediction of the target feature was within +/-2 goals of the actual value.  

The baseline model with the best results was the Support Vector Regression model, with min-max scaling applied to the data.  With the most effective model identified, hyperparameter tuning was completed using a pipeline and grid search with five folds of cross-validation.  Tuning the SVR model with the best hyperparameters from the results of the grid search led to marginal improvements in model performance.  

Even with hyperparameter tuning and the +/-2 goal margin of error, the optimized SVR model was only able to accurately predict the target feature for 38.3% of the test datapoints.  The corresponding R2 score for this model was 0.629, and the MSE was 49.1.  The performance of the optimized model was not as strong as I had hoped, but there are some next steps detailed below that might help boost the prediction accuracy.  

*See the Modeling.ipynb notebook for details.*


## 🚶 Next Steps
- [ ] Create an ordinally encoded feature that gives higher weighting to the entries that did not originally contain null values (the entries with null values were for the seasons prior to 1997-1998)
- [ ] Engineer a feature that captures a player's goal output relative to the output of their teammates for a given season - this could help provide the model with the desired context about the quality of a player's teammates, which I hoped to achieve by encoding the franchise names into individual features
- [ ] Try running the full cleaned dataset, including the encoded franchise names, through a neural network and see if it can derive some additional context through deep learning, and achieve a higher prediction accuracy
- [ ] One of the potential use cases envisioned for this model was to help sports bettors place winning bets on how many goals a player will score in the coming season.  Though the model's prediction accuracy is not as high as hoped, the predictions can be compared to sportsbook odds to score its effectiveness as a sports betting tool.