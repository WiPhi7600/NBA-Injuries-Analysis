# NBA-Injuries-Analysis
This README provides an overview of the NBA-Injury-Analyses project contained within this repository. This project consists of web-scraping (from multiple sources), intensive data cleaning, data processing (merging datasets, feature engineering), and the creation of visualizations to generate useful insights. 

## Table of contents
* [Motivation](#motivation)
* [Objective](#objective)
* [Data Sources](#data-sources)
* [What Makes This NBA Injury Analysis Unique](#what-makes-this-nba-injury-analysis-unique)
* [Results](#results)
* [Conclusion](#conclusion)
* [Repositiory Structure](#repository_structure)
## Motivation	
The outcome of the 2018/2019 NBA season was largely shaped by a rash of devastating injuries to star players (Kevin Durant, Klay Thompson, DeMarcus Cousins, and Victor Oladipo to name a few). In recent years there **_seems_** to be an increasing number of high profile players experiencing serious injuries (ACL tears, achilles ruptures, lower leg fractures), in turn causing them to miss significant playing time, often to the detriment of their team's success.

![Ouch](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/references/02_images/injury.png)
*Fig.1: (L) Kevin Durant's achilles tear; (R) Klay Thompson's torn MCL in the 2018/2019 NBA Finals*

## Objective
The objective of this project was to determine if serious injuries **_are_** really on the rise in the NBA, and in doing so, explore the nature of NBA injuries during the past decade. 

For the purposes of this analysis, I nominally define "serious injuries" as those causing a player to miss 15 or more games.

## Data Sources
1. **NBA Injury Data**

   I scraped NBA injury data from the website [Prosports Transactions](http://prosportstransactions.com/). This site maintains an open-source archive of sports "transactions", including "the most complete database of pro basketball transactions available". For the purposes of this analysis, two complementary "transaction" types were particularly useful: 
   * **"Missed games due to injury/personal reasons"**
      * This transaction (event) occurs when a player - who is currently on their team's active roster - misses a scheduled game. These missed game events are typically short duration (1-3 games). 
   * **"Movement to/from injured/inactive list (IL)"**
      * This transaction (event) occurs when a team places a player on their inactive roster. Teams typically move a player to the inactive roster if the player is expected to be out for an extended period of time. This allows another (healthy) player to assume the injured player's spot on the active roster.
   
   A "transaction" event in this database provides the following information (see screenshot below):
   *  'Date' - Date of missed game/ movement to IL 
   *  'Team' - The player's team
   *  'Acquired' - The name of the player returning to lineup
   *  'Relinquished' - The name of the player missing a game or placed on the IL
   *  'Notes' - A description of the event/injury
<img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/references/02_images/prosports_transactions_sheetshot.jpg"> 

*Fig.2: Screenshot of search results from Prosports Transactions*

2. **NBA Schedule Data**

   I scraped team schedules (2010-2019) from the website [Basketball Reference](http://basketballreference.com/). This data allowed me to:
   * Determine how many games a player actually missed due to injury
   * Determine when the missed game occurred (game number and season - regular, post, or offseason)
   * Determine the number of back-to-back game sets that occurred each season
   
3. **NBA Player Statistics/Bio Data**

   I also scraped player statistics from the website [Basketball Reference](http://basketballreference.com/), allowing me to:
   * Examine correlations between player age and usage (games played, minutes played) and injury events
   * Constrain analysis to players with a minimum amount of playing times (I chose a 10 minutes per game average); this eliminates noise due to reserve/transient players.

## What makes this NBA injury analysis unique?

While this project is not the first to use [Prosport Transactions](http://prosportstransactions.com/) data to explore injury trends, it is unique in that it **ties both NBA schedule data and player stats/bio to transactions. The merging of injury transactions and schedule data allowed me to determine the number of games missed due to injury, rather than simply looking at the number of "transactions" that occurred**. The latter is an imperfect metric for tracking injuries because it treats all injuries as equal (a sore hamstring != a torn achilles)). Consider the plots below:

![Ouch](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/bar_plot_injury_events.png)
*Fig.3: Count of **injury events (transactions)** each season. [Note: Excludes "non-injury" events including missed games due to personal reasons, rest, sickness; Excludes injuries affecting players averaging less than 10 minutes per game].*


Figure 3, a count of injury events each season, gives the impression that the NBA's injury posture is better today (the 2017 and 2018 seasons) than it was in the recent past (2010-2013). Figure 4, a count of missed games each season due to injury, however presents a very different story, as the 2017 and 2018 seasons appear to have the highest missed games totals in the past 9 seasons.

![Ouch](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/bar_missed_games_all_injuries.png)
*Fig.4: Count of **missed games** each season. [Note: Excludes "non-injury" events including missed games due to personal reasons, rest, sickness; Excludes injuries affecting players averaging less than 10 minutes per game].*

From a team/player/fan's perspective its not the number of injury events that really matters, its the number of games missed. 

## Results 
### Injury Trends
#### Missed Games

Figures 5  provides evidence that the last two seasons have experienced an increase in the number of games missed due to injuries relative to the prior 7 seasons. 

![Ouch](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/bar_missed_games_all_injuries_c2018.png)
*Fig.5: Count of **missed games** each season. [Note: Excludes "non-injury" events including missed games due to personal reasons, rest, sickness; Excludes injuries affecting players averaging less than 10 minutes per game].*

Likewise, Figures 6  provides evidence that the last two seasons have experienced an increase in the number of games missed due to _serious_ injury  (injury duration > 15 games) relative to the prior 7 seasons. 

![Ouch](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/bar_missed_games_serious_injuries_c2018.png)
*Fig.6: Count of **missed games due to serious injuries** each season. [Note: Excludes "non-injury" events including missed games due to personal reasons, rest, sickness; Excludes injuries affecting players averaging less than 10 minutes per game].*

While these plots provide evidence to support my original hypothesis, further data exploration is necessary to  understand why this behavior is occurring. It may simply be due to randomness of injuries (for instance, note that a serious injury at the start of the season will result in more missed games than an injury near the end of the season) or it could a real trend that should give teams and players cause for concern.

#### Injury Types - All Injury Durations

The next thing we can look at is the type of injuries that are occurring. Figure 7 is a word cloud representation of the injury "notes" provided with each injury transaction. 

<p align="center">
   <img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/word_cloud.png">

   *Fig.6: Word cloud of injury "notes" built from 2010-2019 NBA seasons transaction data*
</p>

There were many unique variations of injury notes and as such it was necessary to process the text prior to analysis. For each note I extracted an injury "key word" (a specific body part), and then further group these "key words" into categories (body regions). Figure 7 below displays a stacked bar chart that breaks down missed games by injured "body region". For instance, "lower leg" encompasses shin, calf, tibia, ankle, and achilles injuries. It's clear that knee and lower leg injuries are the primary driver of missed games across all seasons, 2017 and 2018 included. The proportion of injuries appears relatively consistent across all seasons.

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/stacked_bar_missed_games_all_injuries.png)
*Fig.7: Count of **missed games (all injury durations)** each season, broken down by **affected body region**. [Note: excluded events related to personal reasons, rest, sickness; included only players averaging 10 minutes per game].*

We can look at injuries in finer granularity by breaking down body regions in to body parts.  In Figure 8 below, I visually map the count of missed games to specific body parts. Circle size is proportional to the number of missed games.

<img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/hor_bar_all_injuries2018.png" width="400"/><img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/body_map_all_injuries2018.png" width="400"/>

*Fig.8: Mapping of 2018-2019 missed games to specific body parts. Circle size is proportional to number of missed games. [Note: excluded events related to personal reasons, rest, sickness; included only players averaging 10 minutes per game].*

#### Injury Types - Serious Injuries

We can also limit our analysis to serious injuries (more than 15 games missed). In Figure 9 and Figure 10 below, we see that ankle, knee, and foot account for the majority of serious injuries. 

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/stacked_bar_missed_games_serious_injuries.png)
*Fig.9: Count of **missed games due to serious injuries** each season, broken down by **affected body region**. [Note: excluded events related to personal reasons, rest, sickness; included only players averaging 10 minutes per game].*

<img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/hor_bar_serious_injuries2018.png" width="400"/><img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/body_map_serious_injuries2018.png" width="400"/>

*Fig.10: Mapping of 2018-2019 missed games due to serious injury to specific body parts. Circle size is proportional to number of missed games.[Note: excluded events related to personal reasons, rest, sickness; included only players averaging 10 minutes per game].*

Unfortunately these plots do not reveal any unusual trends that might explain the increased in missed games in recent seasons.

#### Player Demographics

One possible explanation for the increase injuries may be that player demographics have changed. Could the league be getting older? Is the average player age increasing? Did players in 2017 and 2018 come into the start of the season with more "wear and tear" (career minutes) than in prior seasons (2010-2016)? One way to answer to these questions is to consider the distribution plots below.

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/distplot_players_older.png)

*Fig.11: Distribution plots for player age and career minutes going in to the 2010-2016 seasons. [Note: Only includes players averaging 10 minutes per game].*

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/distplot_players_recent.png)

*Fig.12: Distribution plots for player age and career minutes going in to the 2017 and 2018 season. [Note: Only includes players averaging 10 minutes per game].*

A visual comparison between Fig 11 and 12 does not reveal any significant differences between 2017/2018 seasons and 2010-2016 seasons

We can also look at the demographics of injured player, to see if there are any interesting trends there.

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/distplot_inj_older.png)

*Fig.13: Distribution plots for player age and career minutes for injuries in the 2010-2016 seasons. [Note: Only includes players averaging 10 minutes per game].*

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/distplot_inj_recent.png)

*Fig.14: Distribution plots for player age and career minutes for injuries in the 2017 and 2018 season. [Note: Only includes players averaging 10 minutes per game].*

Not surprisingly injured players tend to be older and have played more minutes than the league average (Fig.11 and 12). There does not however appear to be a significant difference between the demographics of injured players in 2017/2018 and prior seasons (2010-2016).


#### Rest 

We can also look at the number of games players are resting each season and see how that has changed overtime. In Figure 15 we see a dramatic increase in games missed due to rest in 2015, 2016, and 2017, relative to prior seasons. 

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/bar_missed_games_rest.png)

*Fig.15: Missed games where 'rest' is cited as the reason. [Note: Only includes players averaging 10 minutes per game].*

Interestingly it appears that zero games were missed due to rest in 2018 -  a result I know not to be true. This might reflect a change in how NBA teams are reporting (or not reporting) rest days. The increase in rest games in 2015 and 2016 was widely panned by fans who were unhappy they didn't get to see their favorite players. The absence of reported rest games in 2018 may be the result of NBA teams "disguising" rest games to avoid fan and media backlash. Rather than noting a missed game was due to rest, teams may be choosing to attribute the missed game to some minor "injury".

Intuitively I expected that more rest might lead to fewer injuries, but that does not seem to be the case (see 2017 season).  

#### Schedule

The NBA's schedule can be grueling, particularly during back-to-back sets (two or more consecutive games played on consecutive calendar days). Has the number of back-to-back sets increased in recent years? Figure 16 shows the number of back-to-back sets each season.

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/bar_back_to_backs.png)

*Fig.16:  Back-to-back sets each season. [Note A back-to-back set consists of two or more consecutive games that occur on consecutive calendar days].*

Rather than increasing, it's clear that the number of back-to-back sets have been decreasing. I looked into this further and it appears the NBA accomplished this in part by reducing the number of games in the preseason in order to start the season earlier, while keeping the usual 82 regular season games.

#### Timing of Serious Injuries

Intuitively I expected more serious injures to occur later in the season as players wear down, Figure 17 however suggests that in recent years, the most serious injuries are occurring early in the season. Could this also be related to shortened preseasons and an earlier start to the season? A shortened preseason may be hindering player conditioning, placing them in higher risk for injury at the start of the regular season.  

![Fig](https://github.com/elap733/NBA-Injuries-Analysis/blob/master/results/01_plots/calendar_heat_map_serious_injury.png)

*Fig.17: Heat map of games missed to serious injury events over the course of the season. More specifically this plot shows the number of games missed due to injury events that occurred in in a given month- NOT the number of games missed that month. For instance if a player tore his ACL in November and was out for the
rest of the season, all of those missed games would be counted toward November's total. [Note: excluded events related to personal reasons, rest, sickness; only includes players averaging 10 minutes per game; only includes injuries with injury durations greater than 15 games. Note: 2011 season was a strike shortened season that began in December]*


## Conclusions

This exploratory data analysis of NBA injuries revealed that missed games and serious injuries have increased in recent years. Despite diving deep in to the data, I cannot (yet) say conclusively whether this increase is a real trend that should be cause for concern, or if its just an artifact of injury randomness. The increased number missed games due to serious injuries at the start of the season is an interesting trend, that should be tracked in 2019.

## Repository Structure

### Work Flow
The figure below shows the workflow of scraping, cleaning, and processing that I executed prior to creating visualizations.
<img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/references/02_images/workflow.png" width="600"/>

### Folder Structure
All scripts and data files can be found the appropriate directory below.
<img src="https://github.com/elap733/NBA-Injuries-Analysis/blob/master/references/02_images/folder_structure.png" width="400"/>

