![lolimg](/images/arcanaryze.jpeg)
## Introduction
In order for a team to win a game of League of Legends, they have to destroy the enemy team’s Nexus. In order to work towards that objective, teams claim objectives and try to gather more gold and xp than the other team. Knowing this, we are interested in seeing if we can predict the outcome of the game based on how teams performed during the game. In doing so, we can discover which factors are most important towards a team's success and see which objectives should be prioritized to ensure the highest chance of victory. This leads us to our prediction question:    

### Given post-game data about both team's performance, can we predict which team won or lost?

This prediction question falls under Binary Classification and the response variable is the outcome of the game for each team (win or lose). For our model, we are going to use accuracy to evaluate our model. In comparison to the other metrics such as recall, precision, and F1-score, we feel accuracy would be most fitting as neither false positives or false negatives are more important than the other in our case.
    
Since we are using post-game data, we would have access to all the data presented to us at the time of prediction as no new relevant data would be added after the game is over.

