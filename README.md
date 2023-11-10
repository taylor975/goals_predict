<div align="center">

# 📈 Predicting NHL Goal Scoring 🏒

</div>

## 🎯 Project Overview

The sports analytics industry has been growing steadily since the early 2000’s:  first taking hold in Major League Baseball, but now a prominent feature in every major North American sports league.  In relative terms, the National Hockey League was one of the later adopters of data-driven decision making - ten years ago a franchise with a single data analyst would be considered on the forefront of adoption.  Fast forward to 2023, and over *70%* of the league's franchises have at least three employees in analytics positions (*per reporting by the Athletic, Oct. 2022*).  These days, hockey data is  
being used more than ever to influence operational decision making and with it, the volume and quality of publicly-accessibile data has also risen.

With this in mind, I want to leverage my passion for the NHL, and the bevy of publicly-accessible league data, to answer a burning question I have after years of parking in fantasy hockey leagues: 

**The Big Question:**  Can an ML model (or models) be trained to accurately predict a player’s goal output for a new season, based on their individual characteristics?

This project will look to use machine learning to predict how many goals a given player would be expected to score over a new 82 game-season (within some margin of error).  This is first and foremost a personal interest project, but also has real world applications in player contract valuations and the growing sports betting industry (handicapping player production, team performance).


## 📊 Dataset

The dataset has been collected directly from the NHL's publicly-accessible API, using pull requests from the `people` and `stats` endpoints.  Each row of the resulting dataframe consists of a player's statistics for a single NHL season.  The dataframe includes relevant numerical features, such as:
  - Goals scored
  - Shots on net
  - Shooting percentage
  - Games played
  - Player age
  - Average time on ice (per game)
and categorical features, such as:
  - Team played for
  - Position name & type
  - Year of the season

The unique identifier of a single entry is a combination of the season and a player's name.  It is anticipated that the final dataset will have around 40 unique columns and 30,000 rows.  


## 🚀 Project Workflow

### 🐛 [API Debugging]
Working with API calls was a new experience for me; writing the code needed to collect requisite data was a process that involved calling two different endpoints from the NHL API, setting up conditional statements to execute preliminary feature processing (passing on entries that did not satisfy particular requirements), and debugging error responses (where the response received differed from what was expected).

### 💾 [Data Collection]
Data collection is completed.  In total, just under 38,000 data points were pulled from the NHL's publicly accessible API.  

### 🧹 [Data Cleaning & Prepocessing]
Below is a high-level overview of the data cleaning & preprocessing steps that were taken:
- Evaluated accuracy and completeness of dataset
- Removed all data prior to the 1979-1980 season, which is when the NHL merged with the other professional hockey league in North America, the WHA.  This merger consolidated all of the top talent into one league
- For some features, the NHL only started tracking the data after the 1997-1998 season; missing values were populated with averages from the remainder of the dataset.  Averages were calculated separately for each of the four player positions in the dataset
- Flattened the data such that only one entry exists for each player and season combination ➡️ when the data was collected, players would have multiple entries for a single season if they were traded in the middle of the season
- Made team names consistent.  Some franchises have relocated or simply changed names over the years, I wanted each franchise to only have one team name associated with it.  Current team names were assigned for each franchise
- Converted object datatypes to numerical ➡️ features like player height and time on ice were stored as objects but really contained numerical data
- Encoded categorical data using binary and one-hot encoding, as applicable

### 🔮 [Initial Modeling]
The data was scaled and fit to a linear regression model.  The resulting model was overfit (MSE score of 0.002).  Additional processing and feature engineering needed before running next iteration of modeling. 

## 🚶 Next Steps

- [X] Finish data collection
- [X] Check for duplicates, remove unnecessary columns
- [X] Fix null values
- [ ] Fix target feature (target feature for season 'n' is `goals` from season 'n-1')
- [ ] Adjust scoring stats for era
- [ ] Use trailing seasons to create aggregated features for time-on-ice (percentage change) and main scoring statistics (weighted average)
- [ ] Calculate correlation between features within features matrix, evaluate p-values and manually drop features where correlation is statistically significant


