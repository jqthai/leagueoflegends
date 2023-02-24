# League of Legends - What Top Leagues Around the World are the Most Exciting to Watch?

by James Thai (jqthai@ucsd.edu)

---

## Introduction

In this project, I dove deep into competitive League of Legends matches from around the world. Our dataset contains information from thousands of competitive games, such as the champions selected for each game, the amount of time each game took, the amount of damage a certain player did in a match, and much more insightful data.

For my project, the question I wanted to answer was: 

Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues?

The dataset used to answer this question has 149232 rows and 124 columns. The rows correspond to the individuals of each game in addition to two more for the summary statistics for each team for said game. To answer the question posed above, I specifically looked at the columns 'ckpm', 'kills', and 'gamelength'. The column 'ckpm' stands for 'combined kills per minute' - it contains statistics for the kills per minute for both teams of each game added together. The column 'kills' stands for the amount of kills each team had during the game, and 'gamelength' refers to how long the game lasted (in seconds).

League of Legends is a competitive game that many find extremely fun to experience. Whether you play it or watch it, it is a form of entertainment, and that is why I wanted to answer the question posed above - finding the league that has the most action could be something that people who play and watch League of Legends would like to know.

---

## Cleaning and EDA

### Data Cleaning

Before I was able to start investigating what tier 1 league had the most 'action-packed' games, I cleaned the dataset. In this League of Legends dataset in particular, there were many columns that should have been boolean values but were not. Many of the columns contained information on whether or not a player or team secured the 'first' objective in a game, and it was denoted by 1s and 0s. For example, if we took a look at the 'firstblood' column, it determines whether or not that player or team was secured the first kill of the game. The most appropriate value would be boolean values, so that was the first step I did for cleaning.

The cleaning that was arguably more important for answering my question, however, was creating a new column that determined if the league that each game was played in was classified as a tier 1 league, since my question asks about those leagues specifically. I also cleaned the data by removing the rows that pertained to individual players, since I was only interested in the overall action of a game, not an individual's performance. The columns of data I was interested in were columns of team data that were just repeated.

The top few rows of my cleaned dataframe looks like this (with the cleaned columns and columns of interest):

| firstblood   | firstbloodkill   | firstbloodassist   | firstbloodvictim   |   firstdragon |   firstherald |   firstbaron |   firsttower |   firstmidtower |   firsttothreetowers |   ckpm | tier1   |
|:-------------|:-----------------|:-------------------|:-------------------|--------------:|--------------:|-------------:|-------------:|----------------:|---------------------:|-------:|:--------|
| False        | False            | False              | False              |           nan |           nan |          nan |          nan |             nan |                  nan | 0.9807 | False   |
| True         | False            | True               | False              |           nan |           nan |          nan |          nan |             nan |                  nan | 0.9807 | False   |
| False        | False            | False              | False              |           nan |           nan |          nan |          nan |             nan |                  nan | 0.9807 | False   |
| True         | False            | True               | False              |           nan |           nan |          nan |          nan |             nan |                  nan | 0.9807 | False   |
| True         | True             | False              | False              |           nan |           nan |          nan |          nan |             nan |                  nan | 0.9807 | False   |


