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

Last updated: December 2 2019

##  Where can I actually find football/soccer data?

There are three main ways to get data. You can parse/scrape it from a hobbyist project/website, you can pay for it or you can try to collect it yourself.

Jump to a specific source:

- Open source data on github
  - [openfootball - football.db](#openfootball)
  - [jokecamp/FootballData](#fdata)
  - [soccerstats.us](#soccerstats)
  - [WoSo (Womens) Stats](#woso)
  - [Other smaller projects/repos](#repos)
- Free APIs
  - [football-data.org](#footballdata) (RESTful API)
  - [Sports Open Data](#SportsOpenData) (RESTful API)
  - [openfooty API](#openfooty) (hard to get an API Key)
  - [betlines ninja API](#betlinesNinja) (RESTful API on Mashape)
  - [API-Football](#apifootball)
  - [ScoreBat Video API](#scorebat)
- Commercial APIs
  - [football-api.com](#footballapi)
  - [CrowdScores and FastestLiveScores API](#CrowdScores)
  - [SPAPI](#spapi)
  - [SportMonks.com](#sportmonks)
  - [XMLSoccer.com](#xmlsoccer)
  - [Resultados de Fútbol API](#resultados)
  - [opta](#opta)
  - [prozone](#prozone)
  - [Match Analaysis](#matchanalysis)
  - [apifootball.com](#apifootball)
  - [ElenaSport.io](#elenasport)
  - [SportDeer.com](#sportdeer)
  - [fantasysportnet.com](#fantasysportnet)
  - [Goalserve.com](#goalserve)
  - [Soccer's API](#soccersapi)
- Other Websites
  - [footballsquads.co.uk](#footballsquads.co.uk)
  - [Rec.Sport.Soccer Statistics Foundation (RSSSF)](#rsssf)
  - [football-data.co.uk](#football-data.co.uk)
  - [european-football-statistics.co.uk](#european-football-statistics.co.uk])
  - [openligadb.db](#openligadb.db)
  - [Wikipedia](#wiki)
  - [Post War English & Scottish Football League A - Z Player's Database](#postwar)
  - [2015 Women's World Cup Data](#worldcup2015)
  - [World Cup 2014 APIs](#worldcup)
  - [Deprecated/Retired - "The Graveyard of APIs](#api-graveyard)
  - [More Reading / Resources / Links](#other)
  - [Fantasy Data](#fantasy)


<a name="github"></a>
###  Open Source Data on Github

<a name="openfootball"></a>
#### openfootball - aka football.db

**[openfootball (aka football.db)](http://openfootball.github.io/)** has started a free, open source public domain football database.
The data is historical data, meaning no lives scores but the data does include the schedule, teams and players for the 2014 World Cup along with global league data. This is a very promising project and has the **potential to be the definitive source for historical data for the public**.
See the [opensport Google Group](https://groups.google.com/forum/#!forum/opensport) for discussion and questions. The data is stored in various repos on github.
Consider contributing any data you have yourself and be sure to thank [Gerald Bauer](https://github.com/geraldb). All the various repos can be intimidating. A good place to start is at [github.com/openfootball](https://github.com/openfootball/).

An example of the plain text (custom format):

    [Sat Aug/16]
      12.45  Manchester United    1-2  Swansea City
      15.00  Leicester City       2-2  Everton FC
      15.00  Queens Park Rangers  0-1  Hull City
      15.00  Stoke City           0-1  Aston Villa


Check out the following github organizations for more listings:

 - <https://github.com/footballcsv>
 - <https://github.com/openfootball>
 - <https://github.com/rsssf>
 - <https://github.com/footballdata>

There is also an open source football.db HTTP JSON(P) [API](http://openfootball.github.io/docs/json-api-intro.html) for demo purposes. Example Endpoints:

    http://footballdb.herokuapp.com/api/v1/event/world.2014/teams
    http://footballdb.herokuapp.com/api/v1/event/world.2014/round/1

<a name="fdata"></a>
#### jokecamp/FootballData

[jokecamp/FootballData](https://github.com/jokecamp/FootballData) - my own hodgepodge of JSON and CSV Football/Soccer data on GitHub with a focus on the EPL

<a name="soccerstats"></a>
#### soccerstats.us

**[soccerstats.us](http://soccerstats.us)** is github [organization](https://github.com/soccerstatsus) with multiple repositories for sets of data (with a focus on North American data). The [parser](https://github.com/SoccerStatsUS/parse) is written in python and looks like it was designed to parse the rsssf.com text data.

<a name="woso"></a>
#### WoSo Stats

Women's Soccer Stats. Collecting, analyzing, and sharing data about women's soccer from around the world. [WoSo Stats Homepage](https://wosostats.wordpress.com/) and their github [amj2012/wosostats](https://github.com/amj2012/wosostats). [Browse their data app](https://amj2012.shinyapps.io/wosostats/).

<a name="repos"></a>
#### Other smaller Projects/Repos

 - [github.com/jalapic/engsoccerdata](https://github.com/jalapic/engsoccerdata) includes a [csv file](https://github.com/jalapic/engsoccerdata/blob/master/engsoccerdata.csv) of the top 4 tier English League Soccer games from 1888 to 2014.
 - [planetopendata/awesome-football](https://github.com/planetopendata/awesome-football) includes another list of repos
 - [architv/soccer-cli](https://github.com/architv/soccer-cli) - A command line interface for retrieving football scores.
 - [pssguy/epldata](https://github.com/pssguy/epldata) - Datasets of the English Premier League 1992-2018
 - [jalapic/engsoccerdata](https://github.com/jalapic/engsoccerdata) - English and European soccer results 1871-2017. Includes three English ones (League data, FA Cup data, Playoff data), several European leagues (Spain, Germany, Italy, Holland, France, Belgium, Portugal, Turkey, Scotland, Greece) as well as South Africa and MLS.

<a name="free"></a>
###  Free APIs

<a name="footballdata"></a>
#### football-data.org (beta)

[football-data.org](http://www.football-data.org/) is a RESTful API in beta with regularly updated data. If you [register for a free API key](http://www.football-data.org/register) you will get CORS support. I recommend registering for a key to show your support and help the service track usage. However, a key is not required yet so you can try out the endpoints right now. I am excited to see this API grow and mature!

Available endpoints

    /soccerseasons/
    /soccerseasons/{id}/ranking
    /soccerseasons/{id}/fixtures
    /fixtures
    /soccerseasons/{id}/teams
    /teams/{id}
    /teams/{id}/fixtures/

Some example calls:

- <http://api.football-data.org/v1/soccerseasons>
- <http://api.football-data.org/v1/soccerseasons/351/teams>
- <http://api.football-data.org/v1/fixtures>
- <http://api.football-data.org/v1/teams/5>
- <http://api.football-data.org/v1/soccerseasons/424> (European Championships France 2016)

Example JSON output for a team:

    {
       "_links":{
          "self":{
             "href":"http://api.football-data.org/v1/teams/5"
          },
          "fixtures":{
             "href":"http://api.football-data.org/v1/teams/5/fixtures"
          },
          "players":{
             "href":"http://api.football-data.org/v1/teams/5/players"
          }
       },
       "name":"FC Bayern München",
       "code":"FCB",
       "shortName":"Bayern",
       "squadMarketValue":"559,100,000 €",
       "crestUrl":"http://upload.wikimedia.org/wikipedia/commons/c/c5/Logo_FC_Bayern_München.svg"
    }

<a name="SportsOpenData"></a>
### Sports Open Data API

This free RESTful [API](https://market.mashape.com/sportsop/soccer-sports-open-data) (hosted on Mashape) is an impressive service provided by [http://sportsopendata.net/](http://sportsopendata.net/). Introduced in early 2016 it is very promising. The data is under a [Creative Commons license](http://sportsopendata.net/updates/sports-open-data-creative-commons-license/). Limited to 10,000 requests a month. Mashape registration is required.

Available Endpoints:

    /leagues
    /leagues/{league_slug}/seasons
    /leagues/{league_slug}/seasons/{season_slug}/managers
    /leagues/{league_slug}/seasons/{season_slug}/referees
    /leagues/{league_slug}/seasons/{season_slug}/topscorers
    /leagues/{league_slug}/seasons/{season_slug}/rounds/{round_slug}/matches
    /leagues/{league_slug}/seasons/{season_slug}/rounds/{round_slug}/matches/{match_slug}
    /leagues/{league_slug}/seasons/{season_slug}
    /leagues/{league_slug}/seasons/{season_slug}/rounds
    /leagues/{league_slug}/seasons/{season_slug}/rounds/{round_slug}
    /leagues/{league_slug}/seasons/{season_slug}/standings
    /leagues/{league_slug}/seasons/{season_slug}/standings/{position}
    /leagues/{league_slug}/seasons/{season_slug}/teams
    /leagues/{league_slug}/seasons/{season_slug}/teams/{team_slug}/players
    /stadiums

Example Season JSON

    {
      "identifier": "0e5b67aa8cff15b2c6781454e55d1609",
      "league_identifier": "bc425f51d5ee924580c35c38da138de8",
      "season_slug": "15-16",
      "name": "2015-2016",
      "season_start": "2015-07-01T00:00:00+0200",
      "season_end": "2016-06-30T00:00:00+0200"
    }

Example Standings JSON

    {
        "identifier": "a963a7776379b3c8b818930ae9d9d0ca",
        "position": 1,
        "team_identifier": "a9ef824ba73b0a57e982df21467c3efc",
        "team": "Juventus",
        "overall": {
        "wins": 26,
        "draws": 9,
        "losts": 3,
        "points": 87,
        "scores": 72,
        "conceded": 24,
        "last_5": "WNWWN",
        "macthes_played": 0,
        "goal_difference": 0
    }

<a name="openfooty"></a>
#### openfooty API

[openfooty API](http://www.footytube.com/openfooty/) had promising API documentation but a quick look at the developer forums shows a stale community and questions about why no one seems to actually be able to get a developer key.

<a name="betlinesNinja"></a>
#### Betlines Ninja API (betting/odds)

The free [API](https://market.mashape.com/arisalexis/soccer-odds) (hosted on Mashape) is for betting odds but contains a lot of upcoming fixture data. The API is provided by the [Betlines Ninja](http://betlines.ninja). Along with match data the service provides recent odds data from all major sportsbooks (11 currently including Bwin, Paddy Power, Betfair etc.) Results can be obtained for a maximum of 3 days back in the free plan. There is also a data-dump database of historical data for sale.

<a name="apifootball"></a>
### API-Football

[API-Football](https://www.api-football.com/) covers major and minor football leagues and many more are pending. They ask for league suggestions. Livescore, pre-matchs odds, events, standings, we all put in our API. Free tier is 50 requests a day. More [pricing details](https://www.api-football.com/pricing/). Also on [mashape](https://market.mashape.com/gaudinjeremy/winbot-soccer-api)

Curl Example:

```
curl --get --include 'https://api-football-v1.p.mashape.com/seasons' \
   -H 'X-Mashape-Key: XXXXXXXXXXXXXXXXXXXXXXXXX' \
   -H 'Accept: application/json'
```

<a name="commerical"></a>
### Subscription Services/APIs

<a name="elenasport"></a>
#### Sportdeer.com

[ElenaSport.io](https://elenasport.io) offers historical&live data of over 140 soccer leagues. The service is also pretty cheap, with plans starting from 0€/month. The service is entirely hosted in [RapidApi](https://rapidapi.com/mararrdeveloper/api/elenasport-io1) for the users' convenience. Find out more on the documentation page [API Documentation](https://elenasport.io/documentation/getting_started/)

curl example:
```
curl --location --request GET 'https://elenasport-io1.p.rapidapi.com//v1/fixtures/14143' \
--header 'x-rapidapi-key: $$$_YOUR_SECRET_RAPID_API_TOKEN_$$$' \
```
 
Response example:
```
{
    "results": [
        {
            "id": 14143,
            "idSeason": 435,
            "seasonName": "2019/2020",
            "idHome": 909,
            "homeName": "Parma",
            "idAway": 854,
            "awayName": "Juventus",
            "idStage": 131,
            "idVenue": 709,
            "venueName": "Stadio Ennio Tardini",
            "date": "2019-08-24 16:00:00",
            "status": "finished",
            "round": 1,
            "attendance": null,
            "team_home_90min_goals": 0,
            "team_away_90min_goals": 1,
            "team_home_ET_goals": 0,
            "team_away_ET_goals": 0,
            "team_home_PEN_goals": 0,
            "team_away_PEN_goals": 0,
            "team_home_1stHalf_goals": 0,
            "team_away_1stHalf_goals": 1,
            "team_home_2ndHalf_goals": 0,
            "team_away_2ndHalf_goals": 0,
            "elapsed": 0,
            "elapsedPlus": 0,
            "eventsHash": "15ec7bf0b50732b49f8228e07d24365338f9e3ab994b00af08e5a3bffe55fd8b",
            "lineupsHash": "878f32f76b159494f5a39f9321616c6068cdb82e88df89bcc739bbc1ea78e1f9",
            "statsHash": "6a4875ddaceaa91fb3369f0f6d962f77442daf1b1d97733457d12bcabdf79441",
            "referees": [
                {
                    "type": "referee",
                    "idReferee": 8309,
                    "refereeName": "F. Maresca"
                },
                {
                    "type": "assitant referee",
                    "idReferee": 8323,
                    "refereeName": "P. De Meo"
                },
                {
                    "type": "assitant referee",
                    "idReferee": 8114,
                    "refereeName": "M. Piccinini"
                }
            ],
            "events": [
                {
                    "id": 135422,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 12000,
                    "player1Name": "G. Chiellini",
                    "idPlayer2": null,
                    "player2Name": null,
                    "elapsed": 21,
                    "elapsedPlus": 0,
                    "type": "goal"
                },
                {
                    "id": 135423,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 909,
                    "teamName": "Parma",
                    "idPlayer1": 15892,
                    "player1Name": "D. Kulusevski",
                    "idPlayer2": null,
                    "player2Name": null,
                    "elapsed": 27,
                    "elapsedPlus": 0,
                    "type": "y_card"
                },
                {
                    "id": 135424,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 909,
                    "teamName": "Parma",
                    "idPlayer1": 15893,
                    "player1Name": "Hernani",
                    "idPlayer2": null,
                    "player2Name": null,
                    "elapsed": 45,
                    "elapsedPlus": 0,
                    "type": "y_card"
                },
                {
                    "id": 135425,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 12278,
                    "player1Name": "S. Khedira",
                    "idPlayer2": null,
                    "player2Name": null,
                    "elapsed": 53,
                    "elapsedPlus": 0,
                    "type": "y_card"
                },
                {
                    "id": 135457,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 909,
                    "teamName": "Parma",
                    "idPlayer1": 8132,
                    "player1Name": "L. Siligardi",
                    "idPlayer2": 15892,
                    "player2Name": "D. Kulusevski",
                    "elapsed": 57,
                    "elapsedPlus": 0,
                    "type": "subst"
                },
                {
                    "id": 135458,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 15073,
                    "player1Name": "A. Rabiot",
                    "idPlayer2": 12278,
                    "player2Name": "S. Khedira",
                    "elapsed": 63,
                    "elapsedPlus": 0,
                    "type": "subst"
                },
                {
                    "id": 135459,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 12193,
                    "player1Name": "J. Cuadrado",
                    "idPlayer2": 15894,
                    "player2Name": "Douglas Costa",
                    "elapsed": 71,
                    "elapsedPlus": 0,
                    "type": "subst"
                },
                {
                    "id": 135460,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 909,
                    "teamName": "Parma",
                    "idPlayer1": 11893,
                    "player1Name": "A. Grassi",
                    "idPlayer2": 8478,
                    "player2Name": "G. Brugman",
                    "elapsed": 77,
                    "elapsedPlus": 0,
                    "type": "subst"
                },
                {
                    "id": 135461,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 12135,
                    "player1Name": "F. Bernardeschi",
                    "idPlayer2": 11848,
                    "player2Name": "G. Higuaín",
                    "elapsed": 83,
                    "elapsedPlus": 0,
                    "type": "subst"
                },
                {
                    "id": 135462,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 909,
                    "teamName": "Parma",
                    "idPlayer1": 15899,
                    "player1Name": "Y. Karamoh",
                    "idPlayer2": 8449,
                    "player2Name": "A. Barillà",
                    "elapsed": 85,
                    "elapsedPlus": 0,
                    "type": "subst"
                },
                {
                    "id": 135426,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 12135,
                    "player1Name": "F. Bernardeschi",
                    "idPlayer2": null,
                    "player2Name": null,
                    "elapsed": 88,
                    "elapsedPlus": 0,
                    "type": "y_card"
                },
                {
                    "id": 135427,
                    "idFixture": 14143,
                    "idSeason": 435,
                    "idTeam": 854,
                    "teamName": "Juventus",
                    "idPlayer1": 12159,
                    "player1Name": "M. Pjanić",
                    "idPlayer2": null,
                    "player2Name": null,
                    "elapsed": 90,
                    "elapsedPlus": 1,
                    "type": "y_card"
                }
            ]
        }
    ],
    "page": 1,
    "itemsPerPage": 20
}
```  
<a name="fantasysportnet"></a>

<a name="sportdeer"></a>
#### Sportdeer.com

[SportDeer.com](https://sportdeer.com) offers three different plans. Basic, Normal and Advanced. Plans range from 5€/month to 15€/month but will be free of charge till the end of 2018. Registration is required. [API Documentation](https://sportdeer.com/documentation/football/api)

Some example endpoints are:

    https://api.sportdeer.com/v1/countries?access_token=***YOUR_ACCES_TOKEN***
    https://api.sportdeer.com/v1/countries/:id?access_token=***YOUR_ACCES_TOKEN***
    https://api.sportdeer.com/v1/leagues?access_token=***YOUR_ACCES_TOKEN***

<a name="fantasysportnet"></a>
#### fantasysportnet

I have not had time to check this out but adding to the list for now. <http://www.fantasysportnet.com/data/home.jsp>

<a name="goalserve"></a>
#### GoalServe.com

Goalserve Soccer Data Feeds API provide live score services, fixtures and results, In-Game player stats, profiles, injuries, historical data, prematch and inplay Odds. More than 400 football leagues in live data coverage. XML and JSON formats. Free Trial period available. Pricing starts at $150+.

<https://www.goalserve.com/en/sport-data-feeds/soccer-api/prices>

<a name="soccersapi"></a>
#### Soccer's API

[Soccer's API](https://soccersapi.com) offers live scores and stats. More than 800 leagues, match statistics, stats of teams and players, squads and player profiles, lineups, information about online broadcasts (DAZN, ESPN...), TV channels and data about Bookmakers with up to 30 of them with pre-match and Inplay odds. [Plans price](https://soccersapi.com/page/pricing) range from $30-$300 USD per month. A 15-day free trial available.

Example for Match by ID - Endpoint Response (Real Madrid vs PSG - 2019-11-26). Url format is <https://api.soccersapi.com/v2.2/fixtures/?user=YOUR_USER&token=YOUR_TOKEN&t=info2&id=11999203>

```
{
  "data": {
      "id": 11965947,
      "status": 3,
      "pitch": null,
      "time": {
          "datetime": "2019-11-26 20:00:00",
          "date": "2019-11-26",
          "time": "20:00:00",
          "minute": null,
          "timestamp": 1574798400,
          "timezone": "UTC"
      },
      "teams": {
          "home": {
              "id": 3468,
              "name": "Real Madrid",
              "short_code": "RMA",
              "img": "https://cdn.soccersapi.com/images/soccer/teams/150/3468.png",
              "form": "4-3-1-2",
              "coach_id": 407775
          },
          "away": {
              "id": 0,
              "name": "Paris Saint Germain",
              "short_code": "PSG",
              "img": "https://cdn.soccersapi.com/images/soccer/teams/150/0.png",
              "form": "4-3-3",
              "coach_id": 523937
          }
      },
      "league": {
          "id": 2,
          "name": "Champions League",
          "type": "cup_international",
          "country_id": 41,
          "country_name": "Republic of Ireland",
          "country_flag": ""
      },
      "scores": {
          "home_score": 2,
          "away_score": 2,
          "ht_score": "1-0",
          "ft_score": "2-2",
          "et_score": null,
          "ps_score": null
      },
      "referee_id": 16678,
      "round_id": 184042,
      "season_id": 16029,
      "stage_id": 77443828,
      "group_id": 242175,
      "aggregate_id": null,
      "winner_team_id": null,
      "venue_id": 2020,
      "standings": {
          "home_position": 2,
          "away_position": 1
      },
      "assistants": {
          "first_assistant_id": 12857,
          "second_assistant_id": 12861,
          "fourth_assistant_id": null
      },
      "colors": {
          "home": {
              "color": "#F0F0F0",
              "kit_colors": "#F0F0F0,#F0F0F0,#F0F0F0,#F0F0F0,#EEEEEE,#EEEEEE,#F0F0F0"
          },
          "away": {
              "color": null,
              "kit_colors": null
          }
      },
      "weather_report": {
          "code": "drizzle",
          "type": "light intensity drizzle",
          "temperature": {
              "fahrenheit": {
                  "temp": 53.83,
                  "unit": "fahrenheit"
              },
              "celcius": {
                  "temp": 12.1,
                  "unit": "celcius"
              }
          },
          "clouds": "75%",
          "humidity": "100%",
          "pressure": 1010,
          "wind": {
              "speed": "8.05 m/s",
              "degree": 180
          },
          "coordinates": {
              "lat": 40.42,
              "lon": -3.7
          },
          "updated_at": "2019-11-26T19:45:04.436942Z"
      }
  },
...
```

<a name="footballapi"></a>
#### football-api

[football-api.com](http://football-api.com/) is a paid API service. The API rxrestricts by IP addresses and limit calls based on your package. Includes endpoints for competitions, teams, standings, live scores, fixtures and commentaries. See the [pricing page](http://football-api.com/pricing/). Prices range from $15 to $200 per month.

Example endpoints:

    api/?Action=competitions&APIKey=xxxx
    api/?Action=standings&comp_id=1204&APIKey=xxxx
    api/?Action=today&comp_id=1204&APIKey=xxxx
    api/?Action=fixtures&comp_id=1024&&match_date=[DATE_IN_d.m.Y_FORMAT]&APIKey=xxxx
    api/?Action=commentaries&APIKey=xxxx&match_id=[MATCH_ID]

The demo (EPL) free feature is no longer available. Here is part of the [email notice](http://football-api.com/?na=v&id=2&nk=276-ef27b4e6d5) sent:

> Discontinuation Of The Demo Plan (March 3 2016)
>
>  In the last couple of weekends, we experienced a very high load on our servers. This lead to delayed responses and very slow data return. We had to take emergency measures and unfortunately, we had to suspend the demo access to the English Premier League in favour of our paying customers. This decision is hard for us since we would like to offer this demo access for free. However, with around 3000 demo users, the load on our servers was too high.

> Since the demo plan was not planned for production purposes (hence the name), we apologize if we have disturbed your development and surprised you with a lot of error messages.


<i>This is why we can't have nice things.</i>

<a name="CrowdScores"></a>
#### CrowdScores and FastestLiveScores API

[CrowdScores](https://crowdscores.com/) is a UK company that uses a crowd-sourcing football data collection process. You sign up for an account and report game events to their servers. They have iPhone and android apps for reporting. The collected data is then available as an API on [FastestLiveScores.com](http://fastestlivescores.com/). They currently offer [three different API tiers](http://fastestlivescores.com/live-scores-api-feed/). Free trial ($0), Basic ($100 per month) and Pro (price unlisted). View the [API documentation](https://api.crowdscores.com/docs/v1#).

Example endpoints and parameters:

    /teams{?round_ids,competition_ids}
    /matches{?team_id,round_ids,competition_id,from,to}
    /competitions
    /rounds{?competition_ids}
    /seasons
    /league-table/{round_id}
    /league-tables?{team_id,round_id,competition_id}
    /football_states
    /events
    /playerstats?{team_ids,round_ids,competition_ids,season_ids}

<a name="spapi"></a>
### SPAPI

[SPAPI](https://spapi.pw?code=jokecamp) offers subscriptions for a RESTful Sports API including live scores, player statistics, betting odds, pre-game data and match event data. The data response look pretty comprehensive.

Pricing:

    Starter Plan is $299 per month
    Growth pla is $499 per month
    Pro Plan is $899 per month

Look at the example of one of the action data objects. It looks like data for a defensive clearance including x,y coordinates.

    {
        "minute": 53,
        "second": 10,
        "team_id": 32,
        "start_x": 20.8,
        "start_y": 69.5,
        "expanded_minute": 57,
        "period": {
          "name": "SecondHalf",
          "value": 2
        },
        "type": {
          "name": "Clearance",
          "value": 12
        },
        "outcome_type": {
          "name": "Successful",
          "value": 1
        },
        "qualifiers": [{
          "type": "Head"
        }, {
          "type": "Zone",
          "value": "Back"
        }],
        "is_touch": true
      }

Example endpoints:

    https://spapi.pw/api/v1/competitions/5/upcoming_matches
    https://spapi.pw/api/v1/competitions/5/finished_matches
    https://spapi.pw/api/v1/competitions/5/standings?standing_type=standings
    https://spapi.pw/api/v1/competitions/5/player_rankings?ranking_type=all
    https://spapi.pw/api/v1/teams
    https://spapi.pw/api/v1/matches/17588/scores
    https://spapi.pw/api/v1/livescores
    https://spapi.pw/api/v1/players/1/statistics

Pretty impressive but this level of detail comes at a price. Authentication method is a API key in querystring.

<a name="sportmonks"></a>
####Sportmonks.com

[https://sportmonks.com](https://sportmonks.com) offers a JSON API. There are plans with prices ranging between 15 to 200 euro's per month. There is also an option for custom plans. This API support lazy loading, meaning you pass parameters in your request to load relationships and nested relationships.

Registration to this API is free and every plan has a 14 day trial.

Some example endpoints are:

    https://soccer.sportmonks.com/api/v2.0/livescores?api_token=__YOURTOKEN__
    https://soccer.sportmonks.com/api/v2.0/leagues?api_token=__YOURTOKEN__
    https://soccer.sportmonks.com/api/v2.0/leagues/{id}?api_token=__YOURTOKEN__
    https://soccer.sportmonks.com/api/v2.0/fixtures/{id}?api_token=__YOURTOKEN__&include=localTeam,visitorTeam,events

And an example response looks like:

```json
{
  "home": {
    "team_id": 39,
    "shots_on_goal": 6,
    "shots_total": 16,
    "fouls_total": 11,
    "corners_total": 5,
    "offsides_total": 5,
    "possesion": 68,
    "yellowcards": 2,
    "redcards": 0,
    "saves": 1,
    "team": {
      "id": 39,
      "name": "Bayern München",
      "logo": "/img/teams/GER/39.png",
      "twitter": "@FCBayern"
    }
  },
  "away": {
    "team_id": 123,
    "shots_on_goal": 1,
    "shots_total": 10,
    "fouls_total": 12,
    "corners_total": 2,
    "offsides_total": 2,
    "possesion": 32,
    "yellowcards": 2,
    "redcards": 0,
    "saves": 5,
    "team": {
      "id": 123,
      "name": "Benfica",
      "logo": "/img/teams/POR/123.png",
      "twitter": "@SL_Benfica"
    }
  }
}
```

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

Open source [Python client for XmlSoccer API](https://github.com/martineastwood/xmlsoccer)

<a name="resultados"></a>
#### Resultados de Fútbol API

[Resultados-futbol.com](http://www.resultados-futbol.com/api) is a spanish football website with live scores and historical data with a database of more than four million matches and hundreds of competitions all around the world. When they decided to make their own mobile apps they created an API to serve both the apps and the website and made it available to developers.

Their API covers a lot from live scores with events to player stats, transfers, betting odds, etc. Chances are that if you find some info on the website it will be available on the API also. It is well documented in spanish only.

Their prices range from 99€ per year for around 1,000 requests per day to 499€ per month for 100k request per day. They offer a one month free trial with full access to the API and 500 request per day. Authentication method is a API key in the querystring.

<a name="opta"></a>
#### opta

[opta](http://www.optasports.com) is one of industry leaders. This is what the tv networks use and likely what the actual football clubs use for [scouting](http://www.optasportspro.com/products/scouting-reports-and-data-feeds.aspx). If only this data were public! Opta used to provide a developer program under the title "Opta Playground" but it seems that the [site](http://www.optasports.com/playground-section.aspx) has been removed and now shows a 404 error. The site used to read "Opta can provide data for programmers wishing to develop a mobile app or website with selected historical data available to download." You had to request permission in an email. I applied and they sent me the xml data set for 10 rounds of games from the start of the 2007/2008 Bundesliga 2. The more detailed game data had either x,y coordinates of game events. A very impressive dataset but it felt more like an advertisement. The data provided I had no interest in and I'm not sure why an indie developer would spend time working on a data set they could never afford. They even track this data point "Spectator on pitch." Read this article [FiveThirtyEight behind the scenes look at how opta tracks data](http://fivethirtyeight.com/features/the-people-tracking-every-touch-pass-and-tackle-in-the-world-cup/) (spoiler: young male gamers).

An example of an "event" in xml

```xml
<Event id="1115853439" event_id="9" type_id="3"
    period_id="1" min="0" sec="19" player_id="21202"
    team_id="1744" outcome="1" x="64.9" y="11.6"
    timestamp="2007-08-19T13:02:08.482" last_modified="2007-08-19T13:02:13">
        <Q id="152113216" qualifier_id="56" value="Right"/>
</Event>
```

<a name="prozone"></a>
#### prozone

[prozone](http://www.prozonesports.com/) is another large commercial data provider.

<a name="matchanalysis"></a>
#### Match Analysis

[Match Analysis](http://matchanalysis.com/) is another large commercial data provider that lists Fox Soccer Channel, US National Team and the MLS among their clients.


<a name="scorebat"></a>
#### ScoreBat Video API

[ScoreBat Video API](https://www.scorebat.com/video-api/) is a free API that provides the embed codes for the videos of the goals and video highlights in real time. It covers most of the main football leagues and tournaments.
The API is super fast and very easy to use and requires no authentication. Check the structure [here](https://www.scorebat.com/video-api/)

<a name="apifootball"></a>
#### APIfootball

[apifootball.com](https://apifootball.com/) is an affordable service with live scores for mobile applications and small websites.
The service covers live scores for cups, football leagues, and intenational matches for all of the major football federations all over the world, along with many minor leagues, allowing you to integrate information regarding the football matches into your websites. Included in the available information are livescores, in play events, results, fixtures, standings and odds from 60+ bookmakers.

You can check [here](https://apifootball.com/documentation/) the API comprehensive and very easy to use [documentation](https://apifootball.com/documentation/)

Their prices range from $25 for the european plan to $50 for worldwide plan. [Here](https://apifootball.com/) is  the complete [list](https://apifootball.com/) of prices and features.

<a name="other"></a>
###  Other Websites

<a name="footballsquads.co.uk"></a>
#### FootballSquads

[footballsquads.co.uk](http://www.footballsquads.co.uk/) has current and historical squad details for clubs (rosters) and national teams from all across the world for many leagues and competitions, including the 2014 World Cup squads.

And example of the squad/roster data:

    Num  Name         Nat	Pos  Height	  Weight	Date of Birth	 Birth Place	 Previous Club
    1    David De Gea ESP	 G	   1.92	   82	     07-11-90	     Madrid	         Atlético Madrid
    2    Rafael       BRA	 D	   1.72	   65	     09-07-90	     Petrópolis	  Fluminense

<a name="rsssf"></a>
#### Rec.Sport.Soccer Statistics Foundation (RSSSF)

[Rec.Sport.Soccer Statistics Foundation](http://www.rsssf.com/) (RSSSF) has massive collection of formatted plain text statistics. [An example of English Premier leagues results](http://www.rsssf.com/tablese/eng2015.html#premier).

Example of the data for table results:

    1.Chelsea                  8   7  1  0  23- 8  22
    2.Manchester City          8   5  2  1  18- 8  17
    3.Southampton              8   5  1  2  19- 5  16
    4.West Ham United          8   4  1  3  15-11  13

and scores:

    Round 1
    [Aug 16]
    Arsenal       2-1 Crystal P
    Leicester     2-2 Everton


<a name="football-data.co.uk"></a>
#### football-data.co.uk

[football-data.co.uk](http://www.football-data.co.uk/data.php) is a betting and odds website that has made a lot of historical league data available as csv files. The data includes results and a lot of betting/odds related data. I have tried to aggregate and clean up the data in the following repo [github.com/jokecamp/FootballData](https://github.com/jokecamp/FootballData)

Leagues and divisions included:

    England Football Results  	Premiership & Divs 1,2,3 & Conference
    Scotland Football Results  	Premiership & Divs 1,2 & 3
    Germany Football Results  	Bundesligas 1 & 2
    Italy Football Results  	Serie A & B
    Spain Football Results  	La Liga (Premera & Segunda)
    France Football Results  	Le Championnat & Division 2
    Netherlands Football Results  	KPN Eredivisie
    Belgium Football Results  	Jupiler League
    Portugal Football Results  	Liga I
    Turkey Football Results  	Ligi 1
    Greece Football Results  	Ethniki Katigoria

The key/legend of all the field abbreviations gives you idea of what is available in the CSV files:

    Div = League Division
    Date = Match Date (dd/mm/yy)
    HomeTeam = Home Team
    AwayTeam = Away Team
    FTHG = Full Time Home Team Goals
    FTAG = Full Time Away Team Goals
    FTR = Full Time Result (H=Home Win, D=Draw, A=Away Win)
    HTHG = Half Time Home Team Goals
    HTAG = Half Time Away Team Goals
    HTR = Half Time Result (H=Home Win, D=Draw, A=Away Win)

    Match Statistics (where available)
    Attendance = Crowd Attendance
    Referee = Match Referee
    HS = Home Team Shots
    AS = Away Team Shots
    HST = Home Team Shots on Target
    AST = Away Team Shots on Target
    HHW = Home Team Hit Woodwork
    AHW = Away Team Hit Woodwork
    HC = Home Team Corners
    AC = Away Team Corners
    HF = Home Team Fouls Committed
    AF = Away Team Fouls Committed
    HO = Home Team Offsides
    AO = Away Team Offsides
    HY = Home Team Yellow Cards
    AY = Away Team Yellow Cards
    HR = Home Team Red Cards
    AR = Away Team Red Cards
    HBP = Home Team Bookings Points (10 = yellow, 25 = red)
    ABP = Away Team Bookings Points (10 = yellow, 25 = red)

<a name="european-football-statistics.co.uk]"></a>
#### european-football-statistics.co.uk

[www.european-football-statistics.co.uk](http://www.european-football-statistics.co.uk/) is a visually dated website but has a lot of historical football data (mostly an overview of league/tournament results) displayed in nice clean HTML tables. Looks like they already have 2014 EPL stats. The site claims "The target of this site is to collect european football statistics which are not easily found on internet."

<a name="openligadb.db"></a>
#### openligadb.db

[openligadb.db](http://www.openligadb.de/Webservices/Sportsdata.asmx) has an old-school windows asmx web service with methods such as "GetGoalsByMatch()"

<a name="wiki"></a>
#### Wikipedia

[Wikipedia](http://www.wikipedia.org/) - has a lot of structured data and is also crowd/public sourced. You can use their API to query then parse the data. It is very fragmented into specific pages making this a good source if you are looking for very specific team/player data. For example here is a table of Manchester United season results <http://en.wikipedia.org/wiki/List_of_Manchester_United_F.C._seasons>.

<a name="postwar"></a>
#### Post War English & Scottish Football League A - Z Player's Database

[Post War English & Scottish Football League A - Z Player's Database](http://www.neilbrown.newcastlefans.com/index.html) contains a lot of HTML tables of "players who appeared for their clubs between 1946/47 and the end of the 2013/14 season and who have now left their clubs." Here is a list of [ex-Manchester United players](http://www.neilbrown.newcastlefans.com/manutd/manutd.html).

Stats included are: NAME, POS, SEASONS, SOURCE, TRANSFERRED TO, APPS, GOALS

<a name="worldcup2015"></a>
###  2015 Women's World Cup Data

[FIFA PDF files](http://www.fifa.com/womensworldcup/organisation/documents/index.html) - includes unformatted data on participating teams, schedules and random statistics

[world-cup-women on github](https://github.com/openfootball/world-cup-women) - plain text file list of teams and schedule

[2015 Women's World Cup Wikipedia page](http://en.wikipedia.org/wiki/2015_FIFA_Women%27s_World_Cup) - includes a great visual bracket view

<a name="worldcup"></a>
###  2014 World Cup APIs

[kimono labs 2014 World Cup Api](https://www.kimonolabs.com/worldcup/explorer) - has a very nice restful API available. Free registration required to access the API. The API has a player, team, club, matches, and player_season_stats endpoints. See the [documentation](https://www.kimonolabs.com/worldcup/docs) and start making calls withe the [API explorer](https://www.kimonolabs.com/worldcup/explorer)

[2014 World Cup JSON API - Soccer for Good](http://softwareforgood.com/soccer-good/) - The API is available now. The author explains that the data is from a scraper so the availability is not guaranteed but should be available throughout the tournament. There are endpoints for [teams](http://worldcup.sfg.io/teams), [matches](http://worldcup.sfg.io/matches), [today](http://worldcup.sfg.io/matches/today), [tomorrow](http://worldcup.sfg.io/matches/tomorrow) and [current](http://worldcup.sfg.io/matches/current). The Ruby on Rails source code is available on [Github](https://github.com/estiens/world_cup_json)

[World Cup in JSON](http://worldcup.sfg.io/) - an open source ruby project available at [github.com/estiens/world_cup_json](https://github.com/estiens/world_cup_json) that scrapes a few sources and combines into an API. API is available at http://worldcup.sfg.io/matches

[Unofficial FIFA.com JSON API for Mobile Apps](http://live.mobileapp.fifa.com/api/wc/matches)  This is unofficial and I wouldn't be surprised if it is protected/unavailable soon. Until then its nice to see data straight from the source. Known endpoints: [matches](http://live.mobileapp.fifa.com/api/wc/matches), [teams](http://live.mobileapp.fifa.com/api/wc/teams) or detailed [match info](http://live.mobileapp.fifa.com/api/wc/match/300186492/en)

<a name="api-graveyard"></a>
###  Deprecated/Retired - "The Graveyard of APIs"

[ESPN API](http://developer.espn.com/docs) has an API for registered users (free). You can get a list of all the players in the EPL. However they are very limited in their data. They restrict all fixtures and scores to "strategic partners." However, you can get lists of players and teams. The **Public API is being retired on Monday, December 8, 2014** [Read the announcement](http://developer.espn.com/blog/read/publicretirement)

[StatsFC](https://statsfc.com/) used to have an restful JSON API of all EPL scores and fixtures. It was about $8 us dollars a month but was recently shut down. See their [official statement](https://statsfc.com/statement). They still offer widgets and they plan on reviving their servies. See their comments at the bottom of the page.

###  Other Reading / Resources

#### opendata.stackexchange forum

[Are there any open datasets for Soccer statistics?](http://opendata.stackexchange.com/questions/1007/are-there-any-open-datasets-for-soccer-statistics) - keep your eye on this open data forum for more answers.

<a name="linkedsoccerdata"></a>
#### Linked Soccer Data

[Linked Soccer Data (pdf)](http://ceur-ws.org/Vol-1026/paper6.pdf) is a white paper on one group's attempt to "create a dataset including reliable information about soccer
events covering as many historical data as available including recent competition
results." Some dead links but worth a skim.

<a name="fantasy"></a>
###  Fantasy Data

- [EPL Fantasy Geek](http://jokecamp.github.io/epl-fantasy-geek/) - Current season EPL Fantasy stats for every player.
- [Fantasy Soccer Data Downloader on github](https://github.com/rasbt/datacollect/tree/master/collect_fantasysoccer) - A simple command line tool to download English Premier League (fantasy) soccer data

###  Even more links to explore

You've made it this far. Why stop now?

- [MLS Player Salary information](http://www.mlsplayers.org/salary_info.html) - 2007 to current salary amounts in pdf from the MLS Players union.
- See this 2012 [opisthokonta.net](http://opisthokonta.net/?page_id=995) blog post.
- [Football Club Elo Ratings Data & Source](http://clubelo.com/Data/)
- [sportscruncher links blog post](http://sportscruncher.wordpress.com/2013/03/01/links-soccer_data/) and [soccer ratings post](http://sportscruncher.wordpress.com/2013/03/14/links-soccer-ratings/)
- [Football kit colors](http://www.colours-of-football.com/)
- [Historial kits](http://www.historicalkits.co.uk/)
- [ENFA - English National Football Archive](http://www.enfa.co.uk/)
- [Association of Football Statisticians (AFS)](http://www.11v11.com/about-afs/)
- [European football squads since 1999](http://en.eufo.de/site/about-us)
- FIFA Video Game Data at [leereilly/fifa-soccer-12-ultimate-team-data](https://github.com/leereilly/fifa-soccer-12-ultimate-team-data)
- [Football Stats and Betting data](http://www.statto.com/football/stats/)


###  Share your own sources -- What have I missed?

Please let me know about your own data sources or add a pull request on [github](https://github.com/jokecamp/jokecamp.com/blob/master/_posts/2014-03-08-guide-to-football-and-soccer-data-and-apis.markdown). I have mainly searched for EPL data and would love to add data from other leagues/competitions to the list.
