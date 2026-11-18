# Case 1
Examine the relationship between the timing of the first tower's destruction and the attacking side's probability of winning.
Identify the first tower to be destroyed, which side it belonged to, and the time at which the event occurred. Correlate this early advantage with the match outcome and subsequent map control.

## Hypotheses
1. If a team destroys the first tower, their probability of winning increases.
2. The impact of destroying the first T1 tower early on the probability of winning differs between the Radiant and Dire teams.
3. The earlier a team destroys the first T1 tower, the higher their probability of winning.

# Data source
mongo__tower_death_events
pg__matches
pg__npcs

# Data Processing
1. Loads all matches from `pg__matches`.
2. Joins events from `mongo__tower_death_events` using `match_id`.
3. Retains only those events with a valid timestamp within the duration of the match.
4. Removes duplicate events based on their `id`.
5. Identifies the first change in the lowest three bits of the mask—this is the logic used to pinpoint the destruction of a T1 tower.
6. Determines which side’s tower was destroyed and which opposing team destroyed it.
7. Produces one row per match; matches with no T1 tower destruction events are excluded.
8. Excludes matches in which towers were destroyed during the same second.

# Methodology
After all data transformations, we group the data by time ranges and then calculate the average win rate (WR) for each side Dire or Radiant.

# Output
mart_first_t1_winrate - which contains all the information on the win rate, we installed this table on Linage and transfer all the values ​​to the superset

# Visualization
<img width="1061" height="503" alt="изображение" src="https://github.com/user-attachments/assets/0a0053fd-c15e-44fb-a6db-635501a257d0" />
img1 Winrate in terms of time

<img width="1046" height="558" alt="изображение" src="https://github.com/user-attachments/assets/695bf632-0596-4f9b-ac4b-aeb9e4a02720" />
img2 Data distribution over a time range

# Hypotheses

## 1.Destroying the first T1 tower increases the attacking sides chances of victory.
Confirmed
## 2.The impact of the first tower
differs between Radiant and
Dire (a difference of ~5%).
Confirmed.
## 3.The earlier a T1 tower is destroyed, the higher the probability of victory (a difference of >5% between the 5-minute and 25-minute marks).
Despite an overall decline in win rate over time, the chance of winning increases sharply after the 25-minute mark.
This is due to a small sample size, as such an occurrence (the first tower falling at 25 minutes) is rare.

# Conclusion
The Impact of Destroying the First Tier 1 (T1) Tower on Victory
313,654 relevant matches were analyzed.
If the first tower is destroyed within the first 5 minutes, the team that destroys it wins in 65.5% of cases.
When it is destroyed between the 15th and 20th minute, the win rate is 60.6%.
A statistical correlation has been identified: the early destruction of the first tower is associated with a higher probability of victory.
