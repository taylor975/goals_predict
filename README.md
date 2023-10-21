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
Data collection is still ongoing and is being ran in batches of 5,000 player IDs at a time.


## 🚶 Next Steps

- [ ] Finish data collection
- [ ] Check for duplicates, remove unnecessary columns
- [ ] Fix null values
- [ ] Add era adjusted scoring feature
- [ ] Adjust values in shortened seasons (lockouts, COVID)


