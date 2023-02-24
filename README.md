# League of Legends - What Top Leagues Around the World are the Most Exciting to Watch?

by James Thai (jqthai@ucsd.edu)

---

## Introduction

In this project, I dove deep into competitive League of Legends matches from around the world. Our dataset contains information from thousands of competitive games, such as the champions selected for each game, the amount of time each game took, the amount of damage a certain player did in a match, and much more insightful data.

For my project, the question I wanted to answer was: 

Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues?

The dataset used to answer this question has 149232 rows and 124 columns. The rows correspond to the individuals of each game in addition to two more for the summary statistics for each team for said game. To answer the question posed above, I specifically looked at the columns 'ckpm', 'kills', and 'gamelength'. The column 'ckpm' stands for 'combined kills per minute' - it contains statistics for the kills per minute for both teams of each game added together. The 'ckpm' column is arguably the most important - it was the column I used to quantify and measure 'action' in these games. The column 'kills' stands for the amount of kills each team had during the game, and 'gamelength' refers to how long the game lasted (in seconds).

League of Legends is a competitive game that many find extremely fun to experience. Whether you play it or watch it, it is a form of entertainment, and that is why I wanted to answer the question posed above - finding the league that has the most action is something that I as a long time League of Legends player would like to know.

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

### Univariate Analysis

The histogram shown below displays the distribution of the 'ckpm' column from our given dataset. In our question, we are using 'ckpm' as a measure of 'action' within these games. In this histogram, we can tell that the average 'ckpm' lies at just at or lower than 0.8. This means that we can expect the average 'ckpm' for tier 1 games to be around this; it informs us that these tier 1 leagues average just under a kill per minute.

<iframe src="assets/ckpm_hist.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

The scatterplot shown below displays the relationship between 'gamelength' and 'ckpm'. Intuitively, one might think that longer games means more action. While this is technically true in the sense that there are more opportunities for action to occur, the scatterplot shows us that 'ckpm' actually goes down as a game gets longer; the rate at which 'action' occurs in games decreases as the minutes go on.

<iframe src="assets/gamelength_ckpm.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

Since the question is interested in 'action' per league for every tier 1 league in League of Legends Esports, it makes sense to group the data by league and determine some aggregate statistics. I decided to go with the mean, and look at the relevant values for each league.

 league   |     ckpm |   gamelength |   kills |
|:---------|---------:|-------------:|--------:|
| VCS      | 1.05357  |      1800.96 | 15.6687 |
| CBLOL    | 0.874236 |      1974.09 | 14.2449 |
| LPL      | 0.839325 |      1893.44 | 13.1202 |
| PCS      | 0.833358 |      1858.03 | 12.6255 |
| LEC      | 0.804729 |      1990.53 | 13.2417 |
| LLA      | 0.803713 |      1989.54 | 13.2594 |
| LJL      | 0.778508 |      1921.77 | 12.1379 |
| LCS      | 0.733371 |      1981.59 | 11.982  |
| LCK      | 0.700287 |      2020.06 | 11.5493 |

The table is sorted by 'ckpm' in decreasing order, so the first row represents the league with the highest 'ckpm', or highest amounnt of 'action'. Interestly enough, VCS, the league with the highest 'ckpm', also has the lowest average gamelength, which is what was predicted earlier in the scatterplot.

---

## Assessment of Missingness

### NMAR Analysis

I do not believe there are some columns in this dataset that NMAR (Not Missing at Random). Our dataset has a lot of missing values, but further inspection reveals that a lot of the columns are team oriented, so many of the columns will have missing values by design (for player rows). Many of the other columns also include information comparing individual players to their counterparts on the otherside. There doesn't seem to be many columns that would have missing values based on what those values are in the first place. Many columns are boolean columns as well, indicating whether or not an event occurred for a team. All in all, these columns' missing data by nature seems to depend on other columns and not actually on the missing values themselves. If I wanted a set of data that could help me determine why a column is missing (MAR), it would be information on whether or not the game had technical difficulties and had to be canceled.

Overall, there does not seem to be a column in this dataset whose missingness mechanism is NMAR.

### Missingness Dependency

I was especially intrigued about the firstblood column and its missingness mechanism. Since firstblood is a boolean column on whether or not that team/player participated in getting the first kill, my immediate thought was that first blood data could be missing if a game ended early due to technical difficulties and data wasn't stored, or that the missingness in the 'firstblood' column was MAR, and that it was dependent on the 'gamelength' column.

I ran a permutation test where I shuffled the gamelength columns and calculated the mean difference in game length for games where firstblood was missing and games where it wasn't against the real observed difference. 

A plot of the empirical distribution of simulated statistics as well as the observed mean difference in game length is shown below.

<iframe src="assets/fig2.html" width=800 height=600 frameBorder=0></iframe>

As you can see, the simulated statistics (blue) hover between -10 and 10 seconds of average difference in game length, while the observed statistic is -40, which we did not see at all (a zero percent chance). With this visualization, we can determine that the missingness in the 'firstblood' column is in fact MAR, and that it is dependent on the 'gamelength' column.

I was also curious to see if the missingness of the 'firstblood' column depended on the 'champion' column. To gain insight on whether or not it did, I ran another permutation test. I did this by shuffling the champions column and calculating the respective test statistic. Since the champion column contains categorial data, the test statistic I used was the Total Variation Distance (TVD). This statistic calculates the distance between the probability distribution of categorical variables.

A plot of the empirical distribution of simulated TVDs as well as the observed TVDs of champions is shown below.


