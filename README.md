# WCT-Competition-Leaderboard
Elo-based player rating for World Chase Tag sport.

# How to Use

I use Google's Colab to operate on Google Sheets. Colab code is written in Python. Google Sheets functions may transfer to Excel with some editing.

### Colab
My Colab code is commented. Follow the suggestions in the comments to operate on your own Google Sheet by name in your Google Drive.

Running the Colab code will take the expected 3 columns of data from the "Data" sheet, and update or add to both the "Ratings" and "Archive" sheets. Running does not clear the Data sheet, you will need to delete anything you've already sent through before pasting more. Archive will be appended to at the end.

### Sheets
The Google Sheet's mandatory tabs are Ratings, Data, and Archive.

"Player Record" is a new tab for analyzing an individual player's stats and competition history. The highlighted cell contains a dropdown for editors to select any player on the Ratings leaderboard.

"Pruned Rankings" is an alternative leaderboard that only includes players with at least 20 chases participated in. This exists to omit players who haven't featured enough to be past their "placement matches". Absolute rank here holds more meaning.

"Copy of Archive" is an expanded Archive that is edited by hand to include some extra data. Much is incomplete.

"Formula Workspace" is essentially the ReadMe for the spreadsheet.

Google Sheet Copy Created 7/5/2024

Only contains data from the Open League (Women's Division will be a separate project)

For Public Use: https://docs.google.com/spreadsheets/d/1_a0rEM4zopBnZQNeFqo_dhBO2vd05Q2iJ2wV1s2ViJE/edit?usp=sharing

# Discussion

This image shows the basic variables set within this Elo system.

![Elo Rating Odds](https://github.com/mhummel06/WCT-Competition-Leaderboard/assets/16521298/8b314336-ecad-442b-bccb-ff6f56793172)


The data set for this project is via manual data entry of official competitions since World Chase Tag 3 (International Event), using publicly available footage.

![Event and Rating Distribution](https://github.com/mhummel06/WCT-Competition-Leaderboard/assets/16521298/3955f110-d64f-499f-837f-62082653b87d)

Unfortunately some official event data is omitted or unavailable due to difficulty acquiring accurate player names and tag times. Known omitted events include WCT 6 Antwerp Chase Off and WCT 6 China. Both Antwerp and China footage are recorded livestreams, missing written team rosters, and mention or graphics indicating players at play. These events are each qualifiers for WCT 6 World Championship, the international final.

### Performance-based Curve
The Softening Curve is the first and primary method of adding granularity to the ratings while data points are few, without disrupting the overall positions of rankings too heavily. This is done by logarithmic curve based on the tag time when between 0 and 19.9 seconds. This curve is subtracted from the points earned by the chaser when they make a successful tag (therefore added to the evader's gain). This will vary rewards of different tags that have the same athlete ratings pitted against each other. There is a small reward for tagging quickly, and a small penalty for tagging slowly. As you can see by the Y values, these will realistically only affect points traded by +1 -> -1 points.

![Granularity1](https://github.com/mhummel06/WCT-Competition-Leaderboard/assets/16521298/821a869f-afd9-4947-89b0-bc4698c7e131)

### Multi-Evasion Protection
With per-chase rating updates, Evaders who score multiple evasions have their elo inflated, causing a large correction in favor of Chaser who inevitably gets the tag. To aid in correcting this, when chase result is a tag, we must reduce rating given to the Chaser. Their odds of success were higher than the rating comparison lets on. When the result is another consecutive evasion, often the evader (with their inflated rating) is getting a greatly diminished return for their point scored. Tag Odds increased 8% (with total chance capped at 96%) per opponent's evasions after the first, reducing rating gain upon successful tag, and increasing value of evasion in succession. (DISCLAIMER: The comparison in this image was done when the odds were adjusted by 12% instead of 8%)

![Multievade sample](https://github.com/mhummel06/WCT-Competition-Leaderboard/assets/16521298/dd1ed993-a2c9-4402-ad92-5b86508b44a1)


## Limitations

Limitations and problems of attempting to quantify player skill using Elo are discussed many places online. The specific issues that should be acknowledged here are as follows:

### Pairing

The format of official competition is teams of up to six players. Each team takes turns selecting a Chaser to compete against the Evader left on the quad. This selectivity as opposed to randomness means that often team captains are selecting the best match of skill to meet their opponent. This will fail to consistently place more skilled players above less skilled until they have been assigned matchups farther from their true skill level, or at least until they play against each other, which will not occur if they come from the same team.


### New Players/Teams

As always, the more data with the same conditions: the better. World Chase Tag organizers occassionally invite rookie teams into international competitions. In this Elo rating system, they will be given a base rating of 1000. The greatest skews in rating occur when impressively unskilled teams are dominated by some few skilled players. A team of true 1000-rating athletes would not give up 5+ evasions in a row against a skilled player. Putting on these official tag events at a financial loss, and the fledgling scale of the global player base do not allow for a wealth of qualifier events to occur, or be recorded and edited for data collection. This will be solved as popularity and participation increase over time.


### LIVE Update per Chase

Ratings are updated after each chase occurs. Tentatively this could allow rating to be split into each role, Chaser and Evader ratings. The flow of the game also does not allow for a successful Chaser who becomes an Evader to return to a rested state between chases. The latter aspect of the game leads me to choose a composite rating from both roles. Unfortunately this means that there are different circumstances at the beginning of the game, where the favored team (winner of Rock Paper Scissors) often chooses to evade first. The differences in player fatigue and objective are also the reason for omitting Sudden Death tiebreaker chases from the record. There may be context-sensitive ways to account for this in further development.
