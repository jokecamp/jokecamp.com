---
author: jokecamp
comments: true
date: 2014-06-28 20:50:15+00:00
layout: post
slug: alternate-way-to-build-your-own-football-db-sqlite-database
title: Alternate way to build your own football.db sqlite database
desc: "Learn how to use build your own soccer/football sqlite database from the openfootball data set."
wordpress_id: 1582
categories:
- Code
tags:
- github
- soccer
- sportsdata
---

Documentation has been moved to <https://github.com/jokecamp/sportdb-build-scripts>

I've preserved the original text below.

* * *

Below I will describe a set or procedures to generate your own database from scratch. I do not believe this is better than the [openfootball/build](https://github.com/openfootball/build) script but I believe it will help clarify any confusion for people that are unfamiliar with ruby and rake.

Requirements:

 - git
 - Ruby and RubyGems
 - SQLite3



### 1) Create a new folder

The new folder will hold the numerous open football repositories. For this example we will use "openfootball." Other instructions use a slightly different folder format. We will execute all commands below inside this directory.

```
mkdir openfootball
cd openfootball
```

### 2) Install sportdb tool

We need to install the sportdb command line tool. We can easily do this with the Ruby package manager [RubyGems](https://rubygems.org/). We need to install the gem [sportdb](https://rubygems.org/gems/sportdb). You can view source code on [github.com/sportdb/sport.db.ruby](https://github.com/sportdb/sport.db.ruby)
Run the following:

`gem install sportdb`

Expected Ouput:

```
Successfully installed sportdb-1.9.2
Parsing documentation for sportdb-1.9.2
Done installing documentation for sportdb after 1 seconds
1 gem installed
```

If you already have sportdb installed you may want to ensure you have the latest with `gem update sportdb`  Whenever the parser is changed (last change was for the addition of goals to the data template) you will need to update sportdb.

### 3) Working with the sportdb command line tool

Now run the following to see your options:
`sportdb --help`

Run:
`sportdb create`
You will now see a new sqlite database file in your new directory called `sport.db`. sport.db is default filename. I will show how to specify a different name elsewhere. You can connect to the database with sqlite3.exe and browse the schema and tables.
From our openfootball dir run:

```
sqlite3 sport.db
// then in the sqlite prompt run:
sqlite> .tables
// the output will look something like this:
lltime_standing_entries  games                     props
alltime_standings         goals                     races
assocs                    grounds                   records
assocs_assocs             group_standing_entries    regions
badges                    group_standings           rosters
cities                    groups                    rounds
continents                groups_teams              runs
countries                 langs                     seasons
event_standing_entries    leagues                   taggings
event_standings           logs                      tags
events                    names                     teams
events_grounds            persons                   tracks
events_teams              places                    usages
```

However, the tables are still empty. We can get a quick snapshot of the database by running `sportdb stats` too see a quick summary of table counts.

```
sportdb stat
// example output
  0 leagues  /  56 seasons
  0 events (league+season recs)  /  0 rounds  /  0 groups
  0 teams
  0 games
  0 badges
  0 tracks / 0 races (track+event recs) / 0 runs
  0 records (race|run+person recs)
  0 rosters (person+team+event recs)
  0 goals (person+game recs)
  0  assocs|orgs
  0  grounds|stadiums
Stats:
     0 continents
     0 countries (0 supras, 0 deps)
     0 regions
     0 metros
     0 cities (0 metros)
     0 districts
     0 places
     0 names
     0 langs
     0 usages
```



### 4) Downloading the raw data from github



At this point we only have the ruby parser. We need to download the raw data stored in various github repos. It important where you put the repos and how you refrence them in the scripts. In this example I will be putting everything in the `openfootball` folder. However, if you are using the official rake file to build you will **need** to have the expected folder structure as described in this [thread](https://groups.google.com/forum/#!searchin/opensport/league$20with$20key$20missing/opensport/TA3a2OUBKMc/H9v-1AaMGK8J).

Our first **required dependency** is [github.com/openmundi/world.db](https://github.com/openmundi/world.db). It contains geographic information including country names and codes.

From inside the `/openfootball` folder run the following:

```
git clone git://github.com/openmundi/world.db.git
```

This creates a new folder in our repo called world.db. Now if we run the setup command
we can setup the database with our world.db data.

```
sportdb setup --worldinclude world.db
```

This time in the output we can see our sportdb parser doing actual work. Here is a snippet of the logs

```
[info] parsing data 'pacific/nz-new-zealand/cities' (world.db/pacific/nz-new-zealand/cities.txt)...
[info] parsing data 'south-america/1-codes/fifa' (world.db/south-america/1-codes/fifa.yml)...
[info] parsing data 'south-america/1-codes/internet' (world.db/south-america/1-codes/internet.yml)...
```

Now run `sportdb stat` and you will see the new data included in the summary. I prefer to delete the existing database and start over. You will need to do this is you are using the `create` or `setup` commands. If you use the `update` command this is not required.

When including files you can use relative paths. Since I am using one folder I can keep this simple, otherwise I would need to have something like `--worldinclude ./world.db`

### 5) Putting it all together. Download and build the entire football.db

The below download.sh script will clone all the current data repos. If the repos already exist you will see errors. You can only clone a repo once. I usually delete the folders and re-download them when I want updated data.

Create **download.sh** and run `sh download.sh`

```
git clone https://github.com/openmundi/world.db.git
git clone https://github.com/openfootball/stadiums.git
git clone https://github.com/openfootball/players.git
git clone https://github.com/openfootball/national-teams
git clone https://github.com/openfootball/world-cup.git
git clone https://github.com/openfootball/euro-cup.git
git clone https://github.com/openfootball/africa-cup.git
git clone https://github.com/openfootball/north-america-gold-cup.git
git clone https://github.com/openfootball/copa-america.git
git clone https://github.com/openfootball/at-austria.git
git clone https://github.com/openfootball/de-deutschland.git
git clone https://github.com/openfootball/en-england.git
git clone https://github.com/openfootball/es-espana.git
git clone https://github.com/openfootball/it-italy.git
git clone https://github.com/openfootball/europe-champions-league.git
git clone https://github.com/openfootball/mx-mexico.git
git clone https://github.com/openfootball/br-brazil.git
git clone https://github.com/openfootball/north-america-champions-league.git
git clone https://github.com/openfootball/copa-libertadores.git
git clone https://github.com/openfootball/copa-sudamericana.git
```

Create **build.sh** and run `sh build.sh`

```
# delete the existing sqlite db
rm football.db

# Setup the database with world data
sportdb --dbname football.db setup --worldinclude world.db

sportdb --dbname football.db update --include 'stadiums'
sportdb --dbname football.db update --include 'players'

# World Cup is dependent on national-teams
sportdb --dbname football.db update --include 'national-teams'
sportdb --dbname football.db update --include 'world-cup'
sportdb --dbname football.db update --include 'euro-cup'
sportdb --dbname football.db update --include 'africa-cup'
sportdb --dbname football.db update --include 'north-america-goldf-cup'
sportdb --dbname football.db update --include 'copa-america'

sportdb --dbname football.db update --include 'world'
sportdb --dbname football.db update --include 'at-austria'
sportdb --dbname football.db update --include 'de-deutschland'
sportdb --dbname football.db update --include 'en-england'
sportdb --dbname football.db update --include 'es-espana'
sportdb --dbname football.db update --include 'it-italy'

sportdb --dbname football.db update --include 'europe-champions-league'

sportdb --dbname football.db update --include 'mx-mexico'
sportdb --dbname football.db update --include 'br-brazil'

sportdb --dbname football.db update --include 'north-america-champions-league'
sportdb --dbname football.db update --include 'copa-libertadores'
sportdb --dbname football.db update --include 'copa-sudamericana'
```

The **order of updates** can be important. See the rake build script for recommended order. https://github.com/openfootball/build/blob/master/settings.rb

Now check the stats for your football.db with
`sportdb --dbname football.db stat`

Example Output

```
  41 leagues  /  56 seasons
  35 events (league+season recs)  /  531 rounds  /  63 groups
  505 teams
  3886 games
  0 badges
  0 tracks / 0 races (track+event recs) / 0 runs
  0 records (race|run+person recs)
  0 rosters (person+team+event recs)
  136 goals (person+game recs)
  0  assocs|orgs
  200  grounds|stadiums
Stats:
    10 continents
   245 countries (1 supras, 48 deps)
   446 regions
     6 metros
  1073 cities (365 metros)
     0 districts
  1780 places
     0 names
     0 langs
```

Discuss and Ask questions for project at [Google Groups](https://groups.google.com/group/opensport)
