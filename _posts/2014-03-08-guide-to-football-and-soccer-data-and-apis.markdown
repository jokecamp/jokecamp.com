---
author: jokecamp
comments: true
date: 2014-03-08 17:08:40+00:00
layout: post
slug: guide-to-football-and-soccer-data-and-apis
title: Guide to Football/Soccer data and APIs
desc: "A list of soccer and football apis, data sets, data services and websites available for developers looking for open data."
wordpress_id: 1369
categories:
- guide
tags:
- api
- opendata
- sportsdata
featured: true
---


Last updated: Oct 26 2014

## Where can I actually find football/soccer data?

There are three main ways to get data. You can parse/scrape it from a hobbyist project/website, you can pay for it or you can try to collect it yourself.

Jump to a specific source:

- Open source data on github
  - [openfootball - football.db](#openfootball)
  - [socerstats.us](#soccerstats)
  - [Other smaller projects/repos](#repos)
- Free APIs
  - [openfooty API](#openfooty) (hard to get an API Key)
- Commercial APIs
  - [football-api.com](#footballapi)
  - [XMLSoccer.com](#xmlsoccer)
  - [opta](#opta)
  - [prozone](#prozone)
  - [Match Analaysis](#matchanalysis)
- Other Websites
  - [footballsquads.co.uk](#footballsquads.co.uk)
  - [Rec.Sport.Soccer Statistics Foundation (RSSSF)](#rsssf)
  - [CrowdScores](#CrowdScores)
  - [football-data.co.uk](#football-data.co.uk)
  - [european-football-statistics.co.uk](#european-football-statistics.co.uk])
  - [openligadb.db](#openligadb.db)
  - [Linked Soccer Data](#linkedsoccerdata) (Research White Paper)
  - [Wikipedia](#wiki)
- [World Cup 2014 APIs](#worldcup)
- [Deprecated/Retired - "The Graveyard of APIs](#api-graveyard)


<a name="github"></a>
### Open Source Data on Github

<a name="openfootball"></a>
#### openfootball - aka football.db

**[openfootball (aka football.db)](http://openfootball.github.io/)** has started a free, open source public domain football database.
The data is historical data, meaning no lives scores but the data does include the schedule, teams and players for the 2014 World Cup along with global league data. This is a very promising project and has the **potential to be the definitive source for historical data for the public**.
See the [opensport Google Group](https://groups.google.com/forum/#!forum/opensport) for discussion and questions. The data is stored in various repos on github.
Consider contributing any data you have yourself and be sure to thank [Gerald Bauer](https://github.com/geraldb). All the various repos can be intimidating. A good place to start is at [github.com/openfootball](https://github.com/openfootball/).

Check out the following github organizations for more listings:

 - <https://github.com/footballcsv>
 - <https://github.com/openfootball>
 - <https://github.com/rsssf>
 - <https://github.com/footballdata>

There is also an open source football.db HTTP JSON(P) [API](http://openfootball.github.io/docs/json-api-intro.html) for demo purposes. Example Endpoints:

    http://footballdb.herokuapp.com/api/v1/event/world.2014/teams
    http://footballdb.herokuapp.com/api/v1/event/world.2014/round/1

<a name="soccerstats"></a>
#### soccerstats.us

**[soccerstats.us](http://soccerstats.us)** is github [organization](https://github.com/soccerstatsus) with multiple repositories for sets of data (with a focus on North American data). The [parser](https://github.com/SoccerStatsUS/parse) is written in python and looks like it was designed to parse the rsssf.com text data.

<a name="repos"></a>
#### Other smaller Projects/Repos

 - [github.com/jalapic/engsoccerdata](https://github.com/jalapic/engsoccerdata) includes a [csv file](https://github.com/jalapic/engsoccerdata/blob/master/engsoccerdata.csv) of the top 4 tier English League Soccer games from 1888 to 2014.
 - [github.com/jokecamp/FootballData](https://github.com/jokecamp/FootballData) includes mostly English JSON and CSV Football/Soccer data for anyone to use.

<a name="free"></a>
### Free APIs

<a name="openfooty"></a>
[openfooty API](http://www.footytube.com/openfooty/) had promising API documentation but a quick look at the developer forums shows a stale community and questions about why no one seems to actually be able to get a developer key.

<a name="commerical"></a>
### Subscription Services/APIs

<a name="footballapi"></a>
#### football-api

[football-api.com](http://football-api.com/) is a paid API service but does offer the English Premier League endpoints for free (demo use). The API will restrict by IP addresses and limit calls based on your package. Includes endponts for Competitions, teams, standings, live scores, fixtures and commentaries. See the [pricing page](http://football-api.com/pricing/).

Example endpoints:

    api/?Action=competitions&APIKey=####
    api/?Action=standings&comp_id=1204&APIKey=####
    api/?Action=today&comp_id=1204&APIKey=####
    api/?Action=fixtures&comp_id=1024&&match_date=[DATE_IN_d.m.Y_FORMAT]&APIKey=####
    api/?Action=commentaries&APIKey=###&match_id=[MATCH_ID]

<a name="xmlsoccer"></a>
#### XMLSoccer.com

[xmlsoccer.com](http://www.xmlsoccer.com/) is another subscription service. You can demo the service for free with the Scottish Premier League. The monthly pricing is 10 &#8364; per 1 month, 25 &#8364; per 3 months and 90 &#8364; per 12 months. You can browse the demo web methods here to see the types of calls available <http://www.xmlsoccer.com/FootballDataDemo.asmx> and the [WSDL](http://www.xmlsoccer.com/FootballDataDemo.asmx?WSDL). The data is only returned in XML.

Each call must provide an API Key. You can get a free demo API Key by registering.

Example results for a team

```xml
<XMLSOCCER.COM>
  <Team>
    <Team_Id>45</Team_Id>
    <Name>Aberdeen</Name>
    <Country>Scotland</Country>
    <Stadium>Pittodrie Stadium</Stadium>
    <HomePageURL>http://www.afc.co.uk</HomePageURL>
    <WIKILink>http://en.wikipedia.org/wiki/Aberdeen_F.C.</WIKILink>
  </Team>
<XMLSOCCER.COM>
```

Player data

```xml
<Player>
  <Id>2523</Id>
  <Name>David Goodwillie</Name>
  <Height>1.7</Height>
  <Weight>70.29</Weight>
  <Nationality>Scotland</Nationality>
  <Position>Forward</Position>
  <Team_Id>45</Team_Id>
  <PlayerNumber>17</PlayerNumber>
  <DateOfBirth>1989-03-28T00:00:00-08:00</DateOfBirth>
  <DateOfSigning>2014-07-07T00:00:00-07:00</DateOfSigning>
  <Signing>Free</Signing>
</Player>
```

#### opta

<a name="opta"></a>

[opta](http://www.optasports.com) is one of industry leaders. This is what the tv networks use and likely what the actual football clubs use for [scouting](http://www.optasportspro.com/products/scouting-reports-and-data-feeds.aspx). If only this data were public! However, [opta Playground](http://www.optasports.com/playground-section.aspx) has a developer program that provides very limited access to historical data. The site reads "Opta can provide data for programmers wishing to develop a mobile app or website with selected historical data available to download." You have to request permission in an email. I applied and they sent me the xml data set for 10 rounds of games from the start of the 2007/2008 Bundesliga 2. The more detailed game data had either x,y coordinates of game events. A very impressive dataset but it felt more like an advertisement. The data provided I had no interest in and I'm not sure why an indie developer would spend time working on a data set they could never afford. Read this article [FiveThirtyEight behind the scenes look at how opta tracks data](http://fivethirtyeight.com/features/the-people-tracking-every-touch-pass-and-tackle-in-the-world-cup/) (spoiler: young male gamers).

<a name="prozone"></a>
#### prozone

[prozone](http://www.prozonesports.com/) is another large commercial data provider.

<a name="matchanalysis"></a>
#### Match Analysis

[Match Analysis](http://matchanalysis.com/) is another large commercial data provider that lists Fox Soccer Channel, US National Team and the MLS among their clients.

<a name="other"></a>
### Other Websites

<a name="footballsquads.co.uk"></a>
#### FootballSquads

[footballsquads.co.uk](http://www.footballsquads.co.uk/) has current and historical squad details for clubs and national teams from all across the world for many leagues and competitions, including the 2014 World Cup squads.

<a name="rsssf"></a>
### Rec.Sport.Soccer Statistics Foundation (RSSSF)

[Rec.Sport.Soccer Statistics Foundation](http://www.rsssf.com/) (RSSSF) has massive collection of formatted plain text statistics. [An example of English Premier leagues results](http://www.rsssf.com/tablese/eng2014.html#premier).

<a name="CrowdScores"></a>
#### CrowdScores

[CrowdScores](https://crowdscores.com/) is UK company trying to crowd-source the football data collection process. You sign up for an account and report game events to their servers. They have web/iphone/android interfaces for reporting. They reward the top reporter with a season ticket. They data collection process is ideal but they might have to work on the incentives. I believe a better incentive would be to allow the reporters who contribute access to an API of all the data collected.

<a name="football-data.co.uk"></a>
#### football-data.co.uk

[football-data.co.uk](http://www.football-data.co.uk/data.php) is a betting and odds website that has made a lot of historical league data available as csv files. The data includes results and a lot of betting/odds related data. I have tried to aggregate and clean up the data in the following repo [github.com/jokecamp/FootballData](https://github.com/jokecamp/FootballData)

<a name="european-football-statistics.co.uk]"></a>
#### european-football-statistics.co.uk

[www.european-football-statistics.co.uk](http://www.european-football-statistics.co.uk/) is a visually dated website but has a lot of historical football data (mostly an overview of league/tournament results) displayed in nice clean HTML tables. Looks like they already have 2014 EPL stats.

<a name="openligadb.db"></a>
#### openligadb.db

[openligadb.db](http://www.openligadb.de/Webservices/Sportsdata.asmx) has an old-school windows asmx web service with methods such as "GetGoalsByMatch()"

<a name="linkedsoccerdata"></a>
#### Linked Soccer Data

[Linked Soccer Data (pdf)](http://ceur-ws.org/Vol-1026/paper6.pdf) is a white paper on one group's attempt to "create a dataset including reliable information about soccer
events covering as many historical data as available including recent competition
results." Some dead links but worth a skim.

<a name="wiki"></a>
#### Wikipedia

[Wikipedia](http://www.wikipedia.org/) - has a lot of structured data. You can use their API to query then parse the data. It is very fragmented into specific pages making this a good source if you are looking for very specific team/player data. For example here is a table of Manchester United season results <http://en.wikipedia.org/wiki/List_of_Manchester_United_F.C._seasons>


#### opendata.stackexchange forum

[Are there any open datasets for Soccer statistics?](http://opendata.stackexchange.com/questions/1007/are-there-any-open-datasets-for-soccer-statistics) - keep your eye on this open data forum for more answers.


<a name="worldcup"></a>
### 2014 World Cup APIs

[kimono labs 2014 World Cup Api](https://www.kimonolabs.com/worldcup/explorer) - has a very nice restful API available. Free registration required to access the API. The API has a player, team, club, matches, and player_season_stats endpoints. See the [documentation](https://www.kimonolabs.com/worldcup/docs) and start making calls withe the [API explorer](https://www.kimonolabs.com/worldcup/explorer)

[2014 World Cup JSON API - Soccer for Good](http://softwareforgood.com/soccer-good/) - The API is available now. The author explains that the data is from a scraper so the availability is not guaranteed but should be available throughout the tournament. There are endpoints for [teams](http://worldcup.sfg.io/teams), [matches](http://worldcup.sfg.io/matches), [today](http://worldcup.sfg.io/matches/today), [tomorrow](http://worldcup.sfg.io/matches/tomorrow) and [current](http://worldcup.sfg.io/matches/current). The Ruby on Rails source code is available on [Github](https://github.com/estiens/world_cup_json)

[World Cup in JSON](http://worldcup.sfg.io/) - an open source ruby project available at [github.com/estiens/world_cup_json](https://github.com/estiens/world_cup_json) that scrapes a few sources and combines into an API. API is available at http://worldcup.sfg.io/matches

[Unofficial FIFA.com JSON API for Mobile Apps](http://live.mobileapp.fifa.com/api/wc/matches)  This is unofficial and I wouldn't be surprised if it is protected/unavailable soon. Until then its nice to see data straight from the source. Known endpoints: [matches](http://live.mobileapp.fifa.com/api/wc/matches), [teams](http://live.mobileapp.fifa.com/api/wc/teams) or detailed [match info](http://live.mobileapp.fifa.com/api/wc/match/300186492/en)

<a name="api-graveyard"></a>
### Deprecated/Retired - "The Graveyard of APIs"

[ESPN API](http://developer.espn.com/docs) has an API for registered users (free). You can get a list of all the players in the EPL. However they are very limited in their data. They restrict all fixtures and scores to "strategic partners." However, you can get lists of players and teams. The **Public API is being retired on Monday, December 8, 2014** [Read the announcement](http://developer.espn.com/blog/read/publicretirement)

[StatsFC](https://statsfc.com/) used to have an restful JSON API of all EPL scores and fixtures. It was about $8 us dollars a month but was recently shut down. There is no doubt it was related to data rights. See their [official statement](https://statsfc.com/statements).

### Share your own sources -- What have I missed?

Please let me know about your own data sources or add a pull request on [github](https://github.com/jokecamp/jokecamp.com/blob/master/_posts/2014-03-08-guide-to-football-and-soccer-data-and-apis.markdown). I have mainly searched for EPL data and would love to add data from other leagues/competitions to the list.
