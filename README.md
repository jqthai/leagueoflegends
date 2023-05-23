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

<iframe src="assets/champ_tvds2.html" width=800 height=600 frameBorder=0></iframe>

As we can see, all of our simulated TVDs are greater than our observed TVD. Because the randomly simulated TVDs were all equal to or greater than the observed one, we can conclude that the missingness of 'firstblood' is not dependent on the champion since the observed value is seen to not be an extreme value that occurs rarely.

## Hypothesis Testing

Going back to the original question, which is:

Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues?

We can perform a hypothesis test to answer these questions. We actually only have to answer the second question, since we answered the first one when we aggregated our data! We said that the VCS was the most 'action-packed' league out of all the tier 1 leagues. Since we aggregated using the mean, our test statistic for this test will be the mean 'ckpm'.

Our hypotheses for this test are:

**Null Hypothesis**: The amount of action in the VCS league is not significantly different than the amount of action than in other leagues.

**Alternative Hypothesis**: The amount of action in the VCS league is significantly higher than the amount of action than in other leagues.

Let's conduct our test with a significance level of α (alpha) = 0.05.

I looked at the number of VCS games that were in the dataset and randomly sampled that amount of games' 'ckpm's, and calculated the mean 'ckpm' for every set of samples, and did it repeatedly, comparing them against our observed mean 'ckpm' of the VCS league.

A histogram of the distribution of simulated mean 'ckpm's against the observed mean 'ckpm' of the VCS league is shown below.

<iframe src="assets/hypothesis_test.html" width=800 height=600 frameBorder=0></iframe>

As we can see, none of the simulated statistics come close to being equal or greater than our observed statistic, leaving us with a p-value of 0.0!

Since our observed p value of 0.0 is less than our significance level of 0.05, we reject the null hypothesis that the amount of action in the VCS league is not significantly different than the amount of action in other tier 1 leagues. Our hypothesis test supports that the amount of action in the VCS league is significantly higher than the amount of action in other tier 1 leagues.

# Predicting Game Outcomes for Tier 1 Professional League of Legends Teams

---

## Framing the Problem

The problem at hand regards the outcome of League of Legends games, specifically Tier 1 Professional League of Legends games. We want to predict whether or not a tier 1 professional League of Legends team will win a given game. 

The prediction problem at hand will be done through classification. More specifically, through the use of a binary classifier, since the response variable we are predicting is one of two outcomes: a win, or a loss.

As an avid League of Legends player and watcher, I wanted to predict whether or not a team would lose to see how different statistics and occurrences during the game affect a team's outcome. Professional League of Legends is extremely different than playing casually or even competitively by yourself online - professional teams study the game and understand the implications of acquiring certain objectives at certain times during games. Creating a model to predict this would be a great classification problem, and the process of building, testing, and improving the model would provide insight to what factors affect winning the most.

The metric I chose to evaluate my model was accuracy, which is simply (# of accurate predictions/# of predictions made). Other suitable metrics, such as recall (# of true positive predictions/# of actual positive instances), or precision (# of true positive predictions/# predicted positive), were also potential candidates for the metric that would be used to be evaluated. I decided to go with arguably the most simple metric, accuracy, because in the case of predicting a win or a loss in a League of Legends game, recall or precision does not seem to be more important or weigh more heavily.

For instance, if our model was instead a COVID test predicting whether or not the user has COVID-19 or not, recall might be prioritized because correctly identifying someone has COVID (true positive) is arguably more important than correctly identifying someone does not (true negative) because we want the positive person to take the proper precautions to maintain their health and not jeopardize the health of others around them.

In the case of precision, there is no tangible difference or worse false prediction (false positive or false negative) when it comes to wins and losses, so just using accuracy would suffice. Additionally, there are an equal amount of wins and losses in the dataset that I built the model on, so there isn't any class imbalance or skew towards wins or losses that we would have to worry about. Overall, accuracy seems to be a perfectly viable metric to use over precision, recall, or other potential metrics.

The dataset that we explore and use to create our binary classifier has *a lot* of data summarizing every game. There are statistics that summarize certain aspects of each game after it has finished, such as total kills, game length, the total number of certain objectives obtained by either team, and much more. Obviously, in the case of our classification problem - we cannot use this data. We want to predict the outcome of a game, and obviously doing so means that we would not have access to the data I just described above - it wouldn't exist while a game is occurring! 

Luckily, our dataset also provides *a lot* of that insightful data that we need! Information on gold difference, xp difference, and other statistics measuring the difference between two teams at the 10 and 15 minute mark into the game is usable for our prediction problem. Other additional information, such as 'firsts', are also extremely insightful. Our dataset provides information on which team accomplished the first of a certain objective. For example, which team secured the first blood/first kill is one of the 'first' statistics. Information that is not aggregate of the entire game and instead indicative of certain timestamps and certain moments as the game occurs is data that we can use to model and predict our outcomes.

---

## Baseline Model

I started off creating a baseline model for the prediction problem. I decided to approach the binary classification problem by using a Decision Tree - a tree that asks and answers questions in regards to the data until it is 'confident' in predicting an outcome.

My baseline model was quite simple - it only included four features. These four features were the gold difference between the team and the opposing team at 15 minutes ('golddiffat15'), the experience difference between the team and the opposing team at 15 minutes, whether or not the team achieved first blood in the game ('firstblood'), and the league that the game was played in.

Two of the features in my baseline model were quantitative - the gold difference and experience difference features. One of my features was categorical - the 'league' column in my dataset. To render this information useful, I had to quantify it. I One Hot Encoded the 'league' column, which essentially determined whether or not the game took place in a specific league for all unique leagues in question by applying 1 for yes and 0 for no. My last feature, 'firstblood', was a binary feature, so it was nominal.

After using training data to train my model, I determined the performance of my model on my testing data. The accuracy my model achieved on the unseen testing data was 0.599, or around 60%. I do not think this current model is necessarily 'good' - it can only predict a little over half of the games correctly consistently. It seems that there is a lot of room for improvement in increasing the accuracy of our model.

---

## Final Model

I added a multitude of features to my model to improve its accuracy. After combing through the data and seeing what information our dataset provided, I decided to use the following features:

#### The original quantitative features, but modified -

'golddiffat15', 'xpdiffat15' transformed into quantiles.

I decided to transform these two columns into percentiles because I thought that seeing the proportion of games the difference in these two metrics (gold, experience) compared to other games would help our model make predictions more than just leaving these two columns of data as is. Every League of Legends game is different, and the gold difference and experience difference at 15 minutes varies a lot. Transforming them into quantiles to compare how big the difference is to a typical League of Legends game could be more useful for our model to measure the impact these two metrics had on a certain game.

#### The rest of the original features in our base model, as is - 

Column names in dataset = 'league', 'firstblood'

These two features seemed to work well in our base model - the one hot encoding for league allows the model to differentiate between leagues and the potential differences in playstyle in different regions. First blood is always an important statistic - it indicates which team dealt the first blow in a given game. Getting the upper hand early in a game is integral to increasing your chances of winning a game, so I decided to keep it as is.

#### More 'firsts' - 

Column names in dataset = 'firstdragon', 'firstbaron', 'firstherald', 'firsttothreetowers'

As with 'firstblood', all other 'firsts' are extremely important as well. Acquiring the first of a certain objective in a given game gives you numerous advantages over the other team, which sets you up for success. In many games, accomplishing these 'firsts' can allow you to 'snowball' a game, which means that your newfound strength over the other team continues to grow at a faster and faster rate because you are continually getting stronger than the other team because of the fact that you are already stronger than them.

In simpler terms - I decided to go with these four columns specifically because they provide different advantages.

Acquiring the 'firstdragon' of a game gives every player on the team bonus stats - whether that be damage, speed, defense, or other more intricate benefits. This gives the team that acquires this an edge over the enemy, increasing their chances of winning.

The same goes for the 'firstbaron' and 'firstherald'. Acquiring the 'firstbaron' means that you get a temporary buff that gives you a great amount of firepower to siege the enemy base, getting closer to victory, and the herald gives the team a strong ally that does the same, but on a smaller scale.

The feature 'firsttothreetowers' is a great indicator for the team that has acquired map control. Having control of the map means having control of the objectives and resources on the map, which greatly contributes to success and victory on the rift.

#### 'side' -

In League of Legends, there are two sides of the map: the Blue and Red Side. Each team starts on one side and tries to siege the side of the enemy team. The map is not symmetrical - certain objectives are catered towards being on a certain side, which means that each side has its own advantages and disadvantages. The blue side is commonly considered the better side - many believe the geometry of the map aids the blue team in securing certain objectives, such as the ones I talked about in the 'firsts' section, like dragons.

#### Standardized difference in kills at 15 minutes - 

This data was not immediately available in the dataset, and I had to do some programming to be able to compute it and use it in my model! I thought that this would be a great feature that would help my model because as tactical as League of Legends is with objectives and map control, getting kills is also extremely important and plays a role in attaining said goals. Killing enemies on the opposing team means more gold, more map control, and more time on the map to secure objectives - all of which are ultimately stepping stones towards victory (or defeat if the team is on the other side of the fighting). I wanted to standardize the data because different games have different amounts of kills, and standardizing it helps mitigate the variance that might occur in the difference of kills at 15 minutes in different games.

I believe all these features helped improve the model of my performance because they provide more insight to what the teams are accomplishing during the game. My base model had features that focused on the laning phase and overall early stages of a game, and the one I improved and just described goes beyond that and includes features that are focused on later stages of the game, which may be more impactful than the beginning of a game.

### Model and Hyperparameters

Using a Decision Tree for my model opens up to more decisions I had to make (that was a great pun) - I had to choose the hyperparameters for which my Decision Tree would use, which are parameters that we tune to improve the accuracy and effectiveness of our model. The hyperparameters in question were 'max_depth', 'min_samples_split', and 'criterion'.

I wanted to find the best hyperparameters for these three because I felt that they would contribute the most to improving my model and making it the best it could possibly be. 

'max_depth' refers to the maximum amount of 'questions' the Decision Tree can ask before having to come to a conclusion. By finding the best number 'max_depth' can be, it allows my model to not overfit to the training data and be able to generalize well to unseen data. It allows my model to find a balance between fitting to the training data but also being able to make accurate predictions on unseen data.

As for 'min_samples_split' and 'criterion' - these two help determine the best way to split the tree into smaller questions. Finding the best value for 'min_samples_split' and 'criterion' would be another great way to improve my model.

The method I used to find the best hyperparameters was by using a GridSearchCV object. What this does is it takes in a list of hyperparameters, and fits itself to a model and training data and computes the accuracy for every single combination of every hyperparameter a certain number of times (can be determined by us). After applying this to my training data and giving my searcher different hyperparameters, the best hyperparameters for my model ended up being:

'max_depth': 5, 'min_samples_split': 100, and 'criterion': 'entropy'

After improving upon my model with many new features and hyperparameters, I put it to the test! The performance my new final model had on the unseen training data had an accuracy of 79% - essentially a 20% improvement in the model compared to the base model!

## Fairness Analysis

If we look back at the model, we can see that it places a lot of emphasis on the 'first' objectives. This is because teams who acquire a majority of the 'first' objectives tend to capitalize on their lead and win the game before the enemy team has a chance to recoup and rally a comeback. 

But is our model as accurate on longer games? The longer a game goes on, the more stable a game is, and more often the two teams are in a stalemate. This means the impact of these 'firsts' diminishes as a game goes on - the opposing team has likely caught up to the team that acquired those 'firsts' by then and negated those advantages. Naturally, one can infer that these 'firsts' are not as important for games that are last longer than average.
In situations where games lasted longer, would our model still be as accurate? 

To answer the question, I did a permutation test with a significance level (alpha) of 0.05 to come to a conclusion.

#### Grouping games into 'long' and 'non-long' (regular) games

I decided to test my model's accuracy on both long games (call this Group X) and non-long (regular games) (call this Group Y). After computing the mean game length and seeing that it was around 1930 seconds (32 minutes), I decided to classify a game as long if it was longer than 2160 seconds (36) minutes.

With this, I decided to evaluate whether or not my model was fair with the metric of accuracy and the test statistic (accuracy of long games - accuracy of non-long (regular) games), which I would expect to be negative.

#### Null Hypothesis

Our model is fair. Its accuracy for games that are long and games that aren't long (regular length) are roughly the same, and any differences are due to random chance.

#### Alternative Hypothesis

Our model is not fair. Its accuracy for games that are longer is lower than its accuracy for games that are not long.

#### Permutation Test Conclusion

After conducting the permutation test, I came up with a p-value of 0.0. This means that none of my simulated differences in accuracy were as low or lower than my observed statistic.

A visualization of the permutation test can be seen below:

<iframe src="assets/modeling_league1.html" width=800 height=600 frameBorder=0></iframe>

According to the visualization above, our observed difference in accuracy was a lot lower than the rest of our simulated statistics, being almost 0.2 (20%) lower for even the closest simulated difference in accuracy.

Since our p-value of 0.0 is less than alpha = 0.05, we reject the null hypothesis that our model is fair and that our model's accuracy for games that are long and games that are not long are roughly the same. Our permutation test supports that the accuracy for long games is significantly less than the accuracy for regular or non-long length games.
