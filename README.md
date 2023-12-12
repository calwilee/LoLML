![lolimg](/images/arcanaryze.jpeg)
## Introduction
In order for a team to win a game of League of Legends, they have to destroy the enemy team’s Nexus. In order to work towards that objective, teams claim objectives and try to gather more gold and xp than the other team. Knowing this, we are interested in seeing if we can predict the outcome of the game based on how teams performed during the game. In doing so, we can discover which factors are most important towards a team's success and see which objectives should be prioritized to ensure the highest chance of victory. This leads us to our prediction question:    

> Given post-game data about both team's performance, can we predict which team won or lost?

This prediction question falls under Binary Classification and the response variable is the outcome of the game for each team (win or lose). For our model, we are going to use accuracy to evaluate our model. In comparison to the other metrics such as recall, precision, and F1-score, we feel accuracy would be most fitting as neither false positives or false negatives are more important than the other in our case.
    
Since we are using post-game data, we would have access to all the data presented to us at the time of prediction as no new relevant data would be added after the game is over.

> A Note on Data Cleaning

Before starting any modeling, we will first clean the dataset briefly. Just like in our Ban Analysis, we are only going to look at games that come from tier one leagues, Worlds, and MSI as they consist of the best players in each region and in the world. We also utilize only the summary rows that describe game outcome and summary statistics of the match. Finally, we chose to drop rows that are labled as incomplete data. Here are the first few rows of the DataFrame we will be working with:

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>gameid</th>
      <th>datacompleteness</th>
      <th>url</th>
      <th>league</th>
      <th>year</th>
      <th>split</th>
      <th>playoffs</th>
      <th>date</th>
      <th>game</th>
      <th>patch</th>
      <th>participantid</th>
      <th>side</th>
      <th>position</th>
      <th>playername</th>
      <th>playerid</th>
      <th>teamname</th>
      <th>teamid</th>
      <th>champion</th>
      <th>ban1</th>
      <th>ban2</th>
      <th>ban3</th>
      <th>ban4</th>
      <th>ban5</th>
      <th>gamelength</th>
      <th>result</th>
      <th>kills</th>
      <th>deaths</th>
      <th>assists</th>
      <th>teamkills</th>
      <th>teamdeaths</th>
      <th>doublekills</th>
      <th>triplekills</th>
      <th>quadrakills</th>
      <th>pentakills</th>
      <th>firstblood</th>
      <th>firstbloodkill</th>
      <th>firstbloodassist</th>
      <th>firstbloodvictim</th>
      <th>team kpm</th>
      <th>ckpm</th>
      <th>firstdragon</th>
      <th>dragons</th>
      <th>opp_dragons</th>
      <th>elementaldrakes</th>
      <th>opp_elementaldrakes</th>
      <th>infernals</th>
      <th>mountains</th>
      <th>clouds</th>
      <th>oceans</th>
      <th>chemtechs</th>
      <th>hextechs</th>
      <th>dragons (type unknown)</th>
      <th>elders</th>
      <th>opp_elders</th>
      <th>firstherald</th>
      <th>heralds</th>
      <th>opp_heralds</th>
      <th>firstbaron</th>
      <th>barons</th>
      <th>opp_barons</th>
      <th>firsttower</th>
      <th>towers</th>
      <th>opp_towers</th>
      <th>firstmidtower</th>
      <th>firsttothreetowers</th>
      <th>turretplates</th>
      <th>opp_turretplates</th>
      <th>inhibitors</th>
      <th>opp_inhibitors</th>
      <th>damagetochampions</th>
      <th>dpm</th>
      <th>damageshare</th>
      <th>damagetakenperminute</th>
      <th>damagemitigatedperminute</th>
      <th>wardsplaced</th>
      <th>wpm</th>
      <th>wardskilled</th>
      <th>wcpm</th>
      <th>controlwardsbought</th>
      <th>visionscore</th>
      <th>vspm</th>
      <th>totalgold</th>
      <th>earnedgold</th>
      <th>earned gpm</th>
      <th>earnedgoldshare</th>
      <th>goldspent</th>
      <th>gspd</th>
      <th>total cs</th>
      <th>minionkills</th>
      <th>monsterkills</th>
      <th>monsterkillsownjungle</th>
      <th>monsterkillsenemyjungle</th>
      <th>cspm</th>
      <th>goldat10</th>
      <th>xpat10</th>
      <th>csat10</th>
      <th>opp_goldat10</th>
      <th>opp_xpat10</th>
      <th>opp_csat10</th>
      <th>golddiffat10</th>
      <th>xpdiffat10</th>
      <th>csdiffat10</th>
      <th>killsat10</th>
      <th>assistsat10</th>
      <th>deathsat10</th>
      <th>opp_killsat10</th>
      <th>opp_assistsat10</th>
      <th>opp_deathsat10</th>
      <th>goldat15</th>
      <th>xpat15</th>
      <th>csat15</th>
      <th>opp_goldat15</th>
      <th>opp_xpat15</th>
      <th>opp_csat15</th>
      <th>golddiffat15</th>
      <th>xpdiffat15</th>
      <th>csdiffat15</th>
      <th>killsat15</th>
      <th>assistsat15</th>
      <th>deathsat15</th>
      <th>opp_killsat15</th>
      <th>opp_assistsat15</th>
      <th>opp_deathsat15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ESPORTSTMNT01_2700815</td>
      <td>complete</td>
      <td>NaN</td>
      <td>LCK</td>
      <td>2022</td>
      <td>Spring</td>
      <td>0</td>
      <td>2022-01-12 06:20:03</td>
      <td>1</td>
      <td>12.01</td>
      <td>100</td>
      <td>Blue</td>
      <td>team</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>DRX</td>
      <td>oe:team:101f8589e58c724c1dcd5a9c1555277</td>
      <td>NaN</td>
      <td>Diana</td>
      <td>Caitlyn</td>
      <td>Twisted Fate</td>
      <td>LeBlanc</td>
      <td>Viktor</td>
      <td>2195</td>
      <td>0</td>
      <td>5</td>
      <td>14</td>
      <td>9</td>
      <td>5</td>
      <td>14</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.1367</td>
      <td>0.5194</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>10.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>50038.0</td>
      <td>1367.7813</td>
      <td>NaN</td>
      <td>2227.5171</td>
      <td>2178.8610</td>
      <td>127.0</td>
      <td>3.4715</td>
      <td>57.0</td>
      <td>1.5581</td>
      <td>55.0</td>
      <td>305.0</td>
      <td>8.3371</td>
      <td>63747</td>
      <td>39978.0</td>
      <td>1092.7927</td>
      <td>NaN</td>
      <td>60525.0</td>
      <td>0.001902</td>
      <td>NaN</td>
      <td>945.0</td>
      <td>287.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>33.6765</td>
      <td>15121.0</td>
      <td>18570.0</td>
      <td>330.0</td>
      <td>14840.0</td>
      <td>18166.0</td>
      <td>324.0</td>
      <td>281.0</td>
      <td>404.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>27198.0</td>
      <td>30325.0</td>
      <td>511.0</td>
      <td>22441.0</td>
      <td>28785.0</td>
      <td>510.0</td>
      <td>4757.0</td>
      <td>1540.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2700815</td>
      <td>complete</td>
      <td>NaN</td>
      <td>LCK</td>
      <td>2022</td>
      <td>Spring</td>
      <td>0</td>
      <td>2022-01-12 06:20:03</td>
      <td>1</td>
      <td>12.01</td>
      <td>200</td>
      <td>Red</td>
      <td>team</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Liiv SANDBOX</td>
      <td>oe:team:c75f1f337fc5867914749d438a4871d</td>
      <td>NaN</td>
      <td>Renekton</td>
      <td>Lee Sin</td>
      <td>Leona</td>
      <td>Jayce</td>
      <td>Akali</td>
      <td>2195</td>
      <td>1</td>
      <td>14</td>
      <td>5</td>
      <td>39</td>
      <td>14</td>
      <td>5</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.3827</td>
      <td>0.5194</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>10.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>59071.0</td>
      <td>1614.6970</td>
      <td>NaN</td>
      <td>2236.7836</td>
      <td>1898.7335</td>
      <td>114.0</td>
      <td>3.1162</td>
      <td>64.0</td>
      <td>1.7494</td>
      <td>43.0</td>
      <td>292.0</td>
      <td>7.9818</td>
      <td>67669</td>
      <td>43900.0</td>
      <td>1200.0000</td>
      <td>NaN</td>
      <td>60410.0</td>
      <td>-0.001902</td>
      <td>NaN</td>
      <td>986.0</td>
      <td>204.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>32.5285</td>
      <td>14840.0</td>
      <td>18166.0</td>
      <td>324.0</td>
      <td>15121.0</td>
      <td>18570.0</td>
      <td>330.0</td>
      <td>-281.0</td>
      <td>-404.0</td>
      <td>-6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>22441.0</td>
      <td>28785.0</td>
      <td>510.0</td>
      <td>27198.0</td>
      <td>30325.0</td>
      <td>511.0</td>
      <td>-4757.0</td>
      <td>-1540.0</td>
      <td>-1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2690695</td>
      <td>complete</td>
      <td>NaN</td>
      <td>LCK</td>
      <td>2022</td>
      <td>Spring</td>
      <td>0</td>
      <td>2022-01-12 09:02:13</td>
      <td>2</td>
      <td>12.01</td>
      <td>100</td>
      <td>Blue</td>
      <td>team</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>DRX</td>
      <td>oe:team:101f8589e58c724c1dcd5a9c1555277</td>
      <td>NaN</td>
      <td>Diana</td>
      <td>Caitlyn</td>
      <td>Yuumi</td>
      <td>Samira</td>
      <td>Syndra</td>
      <td>2070</td>
      <td>0</td>
      <td>7</td>
      <td>15</td>
      <td>21</td>
      <td>7</td>
      <td>15</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.2029</td>
      <td>0.6377</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>66774.0</td>
      <td>1935.4783</td>
      <td>NaN</td>
      <td>2318.4928</td>
      <td>2678.0870</td>
      <td>117.0</td>
      <td>3.3913</td>
      <td>59.0</td>
      <td>1.7101</td>
      <td>60.0</td>
      <td>262.0</td>
      <td>7.5942</td>
      <td>60674</td>
      <td>38182.0</td>
      <td>1106.7246</td>
      <td>NaN</td>
      <td>60660.0</td>
      <td>0.009141</td>
      <td>NaN</td>
      <td>994.0</td>
      <td>186.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>34.2029</td>
      <td>15495.0</td>
      <td>17872.0</td>
      <td>318.0</td>
      <td>16695.0</td>
      <td>19149.0</td>
      <td>333.0</td>
      <td>-1200.0</td>
      <td>-1277.0</td>
      <td>-15.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>23612.0</td>
      <td>29371.0</td>
      <td>528.0</td>
      <td>24657.0</td>
      <td>30106.0</td>
      <td>546.0</td>
      <td>-1045.0</td>
      <td>-735.0</td>
      <td>-18.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2690695</td>
      <td>complete</td>
      <td>NaN</td>
      <td>LCK</td>
      <td>2022</td>
      <td>Spring</td>
      <td>0</td>
      <td>2022-01-12 09:02:13</td>
      <td>2</td>
      <td>12.01</td>
      <td>200</td>
      <td>Red</td>
      <td>team</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Liiv SANDBOX</td>
      <td>oe:team:c75f1f337fc5867914749d438a4871d</td>
      <td>NaN</td>
      <td>Renekton</td>
      <td>Lee Sin</td>
      <td>Twisted Fate</td>
      <td>Viktor</td>
      <td>LeBlanc</td>
      <td>2070</td>
      <td>1</td>
      <td>15</td>
      <td>7</td>
      <td>31</td>
      <td>15</td>
      <td>7</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.4348</td>
      <td>0.6377</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>57616.0</td>
      <td>1670.0290</td>
      <td>NaN</td>
      <td>2938.8116</td>
      <td>2685.8551</td>
      <td>104.0</td>
      <td>3.0145</td>
      <td>60.0</td>
      <td>1.7391</td>
      <td>45.0</td>
      <td>253.0</td>
      <td>7.3333</td>
      <td>67152</td>
      <td>44660.0</td>
      <td>1294.4928</td>
      <td>NaN</td>
      <td>60108.0</td>
      <td>-0.009141</td>
      <td>NaN</td>
      <td>1051.0</td>
      <td>227.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>37.0435</td>
      <td>16695.0</td>
      <td>19149.0</td>
      <td>333.0</td>
      <td>15495.0</td>
      <td>17872.0</td>
      <td>318.0</td>
      <td>1200.0</td>
      <td>1277.0</td>
      <td>15.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>24657.0</td>
      <td>30106.0</td>
      <td>546.0</td>
      <td>23612.0</td>
      <td>29371.0</td>
      <td>528.0</td>
      <td>1045.0</td>
      <td>735.0</td>
      <td>18.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <td>ESPORTSTMNT01_2690705</td>
      <td>complete</td>
      <td>NaN</td>
      <td>LCK</td>
      <td>2022</td>
      <td>Spring</td>
      <td>0</td>
      <td>2022-01-12 10:07:10</td>
      <td>1</td>
      <td>12.01</td>
      <td>100</td>
      <td>Blue</td>
      <td>team</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>T1</td>
      <td>oe:team:ce499dea30cfce118f4fe85da0227e8</td>
      <td>NaN</td>
      <td>Lee Sin</td>
      <td>Ryze</td>
      <td>Viktor</td>
      <td>LeBlanc</td>
      <td>Graves</td>
      <td>2233</td>
      <td>1</td>
      <td>12</td>
      <td>7</td>
      <td>26</td>
      <td>12</td>
      <td>7</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.3224</td>
      <td>0.5105</td>
      <td>1.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>74327.0</td>
      <td>1997.1429</td>
      <td>NaN</td>
      <td>2789.7179</td>
      <td>2453.1214</td>
      <td>122.0</td>
      <td>3.2781</td>
      <td>83.0</td>
      <td>2.2302</td>
      <td>50.0</td>
      <td>332.0</td>
      <td>8.9207</td>
      <td>66455</td>
      <td>42304.0</td>
      <td>1136.6950</td>
      <td>NaN</td>
      <td>59950.0</td>
      <td>0.038682</td>
      <td>NaN</td>
      <td>970.0</td>
      <td>236.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>32.4048</td>
      <td>15662.0</td>
      <td>18130.0</td>
      <td>324.0</td>
      <td>15510.0</td>
      <td>19078.0</td>
      <td>337.0</td>
      <td>152.0</td>
      <td>-948.0</td>
      <td>-13.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>24357.0</td>
      <td>29835.0</td>
      <td>518.0</td>
      <td>23048.0</td>
      <td>30005.0</td>
      <td>533.0</td>
      <td>1309.0</td>
      <td>-170.0</td>
      <td>-15.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table> 

## Baseline Model
![poro](/images/trainporo.jpg_large)

Let’s start by creating a baseline model for prediction. This serves as a good starting point for more advanced modeling techniques. 

> Quantitative Features

- `golddiffat15`
- `xpdiffat15`

> Nominal Features

- `firstdragon` (0 / 1)
- `firstbaron` (0 / 1)
- `firstherald` (0 / 1)
- `firstblood` (0 / 1)
- `firsttothreetowers` (0 / 1)

> Response Variable

- `Result` win(1), lose(0)

All of our nominal data was already encoded as 0 or 1, so we had no need to one hot encode it! 

> Baseline Conclusion 

Our baseline model reached a training accuracy of around 99.9% and a test accuracy of around 84%. The discrepancy between the training and test accuracy is due to our model overfitting to the training data and not being able to generalize as well as we would like. Despite this however, we believe that our model has good performance as it has good accuracy on both the training and test set. In addition to that, it makes intuitive sense that teams with initial gold, experience and objective advantage are more likely to win, making our features that we selected a good base to start with. Lets see if we can take our model a bit further!

## Final Model
![exciteporo](/images/excitedporo.webp) 
Now that we've completed our base model, it is time to improve upon it and create our final model. 
We will choose to keep gold and xp differences, as we feel that these features are integral to predicting game outcomes. After trial and error and pruning our features, we found our baseline model’s nominal features have minimal impact on training accuracy. Therefore, we decided to select different features in replacement of these. 

## Engineered Features

> `soul_type` (Nominal)

In league of legends, the first team to slay 4 dragons gets a soul buff. This buff provides a massive boost in stats to the team that acquires it. We engineered this feature through the drake columns of the data. We will one hot encode this column, but we expect teams that acquire a soul to win much more than teams who do not. 

<div class="table-wrapper" markdown="block">

<iframe src="barchartdrag.html" width=725 height=500 frameBorder=0></iframe>

</div>

Securing a soul greatly improves a team’s chance of winning. 

> `golddiffat15` (Quantitative)

Gold differences should serve as a good measurement of how ahead a team is at the 15 minute mark. Teams with positive gold differences should be stronger than the opponents, and therefore more likely to win the match. We standardized this feature in our feature engineering. 

> `xpdiffat15` (Quantitative)

Similarly to gold differences, the greater of an xp advantage a team has over their opponent, the stronger the team should be. Therefore, we expect teams with greater xp advantages to win more than those with smaller advantages. We standardized this feature in our feature engineering.

## Additional Features

> `inhibitors` (Quantitative)

Inhibitors protect the team's nexuses. This makes this structure a very strong feature to include in our model for a number of reasons. Firstly, a team cannot damage the enemy team's nexus without destroying at least one inhibitor. Therefore a team cannot win if they do not break an inhibitor. This fact alone makes this feature very strong. Secondly, breaking an inhibitor grants a team super minions, effectively forcing one opponent to clear these minions, leaving them stuck in their base for 5 minutes, until the inhibitor respawns. This gives the team a huge advantage in securing objectives, as they essentially will always have a numbers advantage. 

## Algorithm and Hyperparameters

For this model, we chose to use a Random Forest classifier in order to train multiple decision trees on our data and reduce the risk of overfitting. We tuned our hyperparameters with a gridsearch. Our search produced varying results, but we received best test accuracy with a `max_depth` of 12, `n_estimators` of 5 and a entropy as our `criterion`. 

## Baseline Improvements 
We made several improvements to our baseline model improving our test accuracy from 84% to 96%. Our training accuracy did not change significantly. We attribute this improvement to a number of factors: 


> Our baseline model was based purely off of domain knowledge

We selected our features based on our past experiences with League of Legends. With our final model, we ran a number of trials in order to determine which features provided the most impact, as well as examined some features more rigorously. 


> Inhibitors

We came to the realization that inhibitors should be a very strong feature, as a value of 0 in this feature guarantees a loss for the team. Including inhibitors greatly improved our accuracy. 


> Hyperparameters

We did not tune our hyperparameters for our baseline model, compared to the tuning we did for our final model. This is another source of improvement in test accuracy. 


> Feature Engineering

We engineered our own feature, `soul_type`. This feature essentially summarizes the dragon columns. We used `OneHotEncoder()` on this feature. 
We applied `StandardScaler()` to both `golddiffat15` and `xpdiffat15`, as these features had a large range.  

## Fairness Analysis
![fairness](/images/fairness.jpeg){: width="800" }
To finish, we will run a fairness analysis on our model. For this analysis, we will be looking at the model's performance based on the region. Specifically, we will be looking at **South Korea** and **North America** (US and Canada). We selected these two regions because South Korea is considered one of the best in the world while North America is subpar. We are interested in seeing if our model can perform well in different regions despite the skill disparity. We'll still use accuracy to record our model's performance as incorrect and correct predictions still hold the same weight despite the fact that we have limited the regions. 

**Null Hypothesis:** The model performs just as well on both regions.
**Alternative Hypothesis:** The model performs better on one region compare to the other.
**Test Statistic:** Absolute difference between accuracies
**Significance Level:** 0.05

<div class="table-wrapper" markdown="block">

<iframe src="permtest.html" width=725 height=500 frameBorder=0></iframe>

</div>

Because our p-value of 0.324 is greater than our alpha of .05, we fail to reject our null hypothesis. The data provided suggests that our model predicts the outcome with equal accuracy between the South Korea and North America regions










