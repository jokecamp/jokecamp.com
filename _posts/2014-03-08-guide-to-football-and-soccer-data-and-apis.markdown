---
author: jokecamp
comments: true
date: 2014-03-08 17:08:40+00:00
layout: post
slug: guide-to-football-and-soccer-data-and-apis
title: Guide to Football/Soccer data and APIs
wordpress_id: 1369
categories:
- api
- opinion
tags:
- api
- opendata
- sportsdata
---

### Where can I actually find football/soccer data?

**[openfootball](http://openfootball.github.io/)** has started a free (open source) public domain football database. The data is historical data, meaning no lives scores but the data does include the schedule, teams and players for the upcoming 2014 World Cup along with global league data. This is a very promising project and has the **potential to be the definitive source for historical data for the public**. The data is stored in various repos on github. Start browsing and contributing at [github.com/openfootball](https://github.com/openfootball/). See the [opensport Google Group](https://groups.google.com/forum/#!forum/opensport) for discussion and questions.

[footballsquads.co.uk](http://www.footballsquads.co.uk/) has current and historical squad details for clubs and national teams from all across the world for many leagues and competitions, including the 2014 World Cup squads.

[Rec.Sport.Soccer Statistics Foundation](http://www.rsssf.com/) (RSSSF) has massive collection of formatted plain text statistics. [An example of English Premier leagues results](http://www.rsssf.com/tablese/eng2014.html#premier).

[ESPN API](http://developer.espn.com/docs) has an API for registered users (free). You can get a list of all the players in the EPL. However they are very limited in their data. They restrict all fixtures and scores to "strategic partners." However, you can get lists of players and teams.

[opta Playground](http://www.optasports.com/playground-section.aspx) has a developer program that provides very limited access to historical data. The site reads "Opta can provide data for programmers wishing to develop a mobile app or website with selected historical data available to download." You have to request permission in an email. I applied and they sent me the xml data set for 10 rounds of games from the start of the 2007/2008 Bundesliga 2. The more detailed game data had either x,y coordinates of game events. A very impressive dataset but it felt more like an advertisement. The data provided I had no interest in and I'm not sure why an indie developer would spend time working on a data set they could never afford.

[StatsFC](https://statsfc.com/) used to have an restful JSON API of all EPL scores and fixtures. It was about $8 us dollars a month but was recently shut down. There is no doubt it was related to data rights. See their [official statement](https://statsfc.com/statements).

[CrowdScores beta](https://crowdscores.com/) is UK company trying to crowd-source the football data collection process. You sign up for an account and report game events to their servers. They have web/iphone/android interfaces for reporting. They reward the top reporter with a season ticket. They data collection process is ideal but they might have to work on the incentives. I believe a better incentive would be to allow the reporters who contribute access to an API of all the data collected.

[openfooty API](http://www.footytube.com/openfooty/) had promising API documentation but a quick look at the developer forums shows a stale community and questions about why no one seems to actually be able to get a developer key.

[football-data.co.uk](http://www.football-data.co.uk/data.php) has made a lot of historical league data available as csv files. The data includes results and a lot of betting/odds related data. I have tried to aggregate and clean up the data in the following repo [github.com/jokecamp/FootballData](https://github.com/jokecamp/FootballData)

[www.european-football-statistics.co.uk](http://www.european-football-statistics.co.uk/) is a visually dated website but has a lot of historical football data (mostly an overview of league/tournament results) displayed in nice clean HTML tables. Looks like they already have 2014 EPL stats.

[openligadb.db](http://www.openligadb.de/Webservices/Sportsdata.asmx) has an old-school windows asmx web service with methods such as "GetGoalsByMatch()"

[Linked Soccer Data](http://ceur-ws.org/Vol-1026/paper6.pdf) is a white paper on one group's attempt to "create a dataset including reliable information about soccer
events covering as many historical data as available including recent competition
results." Some dead links but worthwhile to skim.

[Are there any open datasets for Soccer statistics?](http://opendata.stackexchange.com/questions/1007/are-there-any-open-datasets-for-soccer-statistics) - keep your eye on this open data forum for more answers.

### 2014 World Cup APIs

[kimono labs 2014 World Cup Api](https://www.kimonolabs.com/worldcup/explorer) - has a very nice restful API available. Free registration required to access the API. The API has a player, team, club, matches, and player_season_stats endpoints. See the [documentation](https://www.kimonolabs.com/worldcup/docs) and start making calls withe the [API explorer](https://www.kimonolabs.com/worldcup/explorer)

[2014 World Cup JSON API - Soccer for Good](http://softwareforgood.com/soccer-good/) - The API is available now. The author explains that the data is from a scraper so the availability is not guaranteed but should be available throughout the tournament. There are endpoints for [teams](http://worldcup.sfg.io/teams), [matches](http://worldcup.sfg.io/matches), [today](http://worldcup.sfg.io/matches/today), [tomorrow](http://worldcup.sfg.io/matches/tomorrow) and [current](http://worldcup.sfg.io/matches/current). The Ruby on Rails source code is available on [Github](https://github.com/estiens/world_cup_json)

[World Cup in JSON](http://worldcup.sfg.io/) - an open source ruby project available at [github.com/estiens/world_cup_json](https://github.com/estiens/world_cup_json) that scrapes a few sources and combines into an API. API is available at http://worldcup.sfg.io/matches

[Unofficial FIFA.com JSON API for Mobile Apps](http://live.mobileapp.fifa.com/api/wc/matches)  This is unofficial and I wouldn't be surprised if it is protected/unavailable soon. Until then its nice to see data straight from the source. Known endpoints: [matches](http://live.mobileapp.fifa.com/api/wc/matches), [teams](http://live.mobileapp.fifa.com/api/wc/teams) or detailed [match info](http://live.mobileapp.fifa.com/api/wc/match/300186492/en)

Please let me know about your own data sources in the comments. I have mainly search for EPL data and would love to add data from other leagues/competitions to the list.
