# Case 3
Analyze pairs of rival heroes and their win rates.
Evaluate hero pairs while accounting for the side (Radiant/Dire), game patch, role, and sample size. Do not draw conclusions based on win rates without considering confidence intervals.

## Hypotheses
1. Some heroes have a higher win rate against specific opponents than their own average win rate.
2. The reliability of identified counter-picks depends on the number of matches played.
3. Pudge has no counter-picks.

# Data source
pg__matches
pg__match_players
pg__heroes

# Data Processing
1. Remove `NULL` values ​​from the source data.
2. Exclude matches with a duration of less than 10 minutes.
3. Retain only 5v5 format matches.
4. Exclude records containing `NULL` values ​​or invalid `account_id`s.
5. Exclude records containing `NULL` values ​​or where `hero_id <= 0`.
6. Add hero names and attributes.
7. Exclude hero pairs that appear in fewer than 30 games.

# Methodology
To begin, we create a PCA model that reveals how frequently—and in what contexts—heroes encounter one another within a vector space. Next, we generate pairs of all heroes and calculate their win rates against each other. Finally, we feed the character vectors, win rates, and the number of games played for each hero pair into a Wilson score interval.

# Output
mart_hero_counterpick_stats - which contains all the information about the characters and key statistics, we installed this table on Linage and transfer all the values ​​to the superset

# Visualization
## Win rate differential for Anti-Mage against all other characters
<img width="1072" height="547" alt="изображение" src="https://github.com/user-attachments/assets/f21e91b9-bda4-43bd-b507-d6f245dd881d" />

## Heat map 
<img width="1052" height="612" alt="изображение" src="https://github.com/user-attachments/assets/b16e213f-358d-4115-a1c5-8830f6b989ef" />

## Distribution of characters by number of matches and the deviation in their win rates
<img width="1085" height="571" alt="изображение" src="https://github.com/user-attachments/assets/1ec2e37d-c5ca-4981-9755-7f2f3d4a8194" />

# Hypotheses

## 1.The reliability of the identified counter-picks depends on the number of matches.
Confirmed
## Counter-picking affects win rate. As can be seen on the heatmap, there are spikes in win rate activity.
Confirmed.
## Pudge has no hard counters. (The win-rate differential against any given hero is 5% or less.)
Confirmed.

# Conclusion
Hero Counter-Pick Analysis
Processed over 309,000 matches and 127 heroes.
Calculated 8,001 hero-pair similarities using PCA and Cosine Similarity.
The final dataset contains 5,286 directed matchups.
For each pair, we calculated the Matchup Win Rate, Baseline Win Rate, and 95% confidence intervals.
Quality checks passed: no invalid win rates or duplicates were found.
