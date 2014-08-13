---
author: jokecamp
comments: true
date: 2012-02-16 01:53:19+00:00
layout: post
slug: importing-dc-capital-bikeshare-data-into-a-sqlite3-database
title: Importing DC Capital BikeShare data into a Sqlite3 database
desc: 'Learn how to import the Washington DC capital bike share data into a sqlite (sqlite3) database'
wordpress_id: 131
categories:
- blog
tags:
- sqlite
---

I recently downloaded and imported all of the available [raw csv files from Capital BikeShare](http://www.capitalbikeshare.com/trip-history-data) into a local sqlite3 database running on my local machine. If you have sqlite3 and you have the sqlite3.exe file already set in your $path environment variable then you may simply run the following command to build a table of all the rides. Once that is done you can start experimenting with different queries.

**Run from command prompt. Creates a new db called bike.db with a table called ride.**

```sqlite3 bike.db < BuildBikeDb.txt```


**Download and save as BuildBikeDb.txt**

```sql

DROP TABLE IF EXISTS ride;
create table ride (Duration text, StartDate , EndDate text, StartStation text, EndStation text, BikeNumber integer, MemberType text);

.separator ','
.import 2010-4th-quarter.csv ride
.import 2011-1st-quarter.csv ride
.import 2011-2nd-quarter.csv ride
.import 2011-3rd-quarter.csv ride
.import 2011-4th-quarter.csv ride

-- Need to add columns, so transfer to temp, create new with addition columns, then copy back over
CREATE TEMPORARY TABLE ride_temp AS select * from ride;
DROP TABLE ride;

create table ride (Duration text, StartYear INTEGER, StartMonth INTEGER, StartDay INTEGER, StartDate text, EndDate text, StartStation text, EndStation text, BikeNumber integer, MemberType text);
INSERT INTO ride (Duration, StartDate, EndDate, StartStation, EndStation, BikeNumber, MemberType) SELECT Duration, StartDate, EndDate, StartStation, EndStation, BikeNumber, MemberType FROM ride_temp;
drop table ride_temp;

create index ind_station on ride(StartStation);
create index ind_station_end on ride(EndStation);
create index ind_startdate on ride(StartDate);

-- delete all header rows that were imported
delete from ride where Duration = 'Duration';

-- Not pretty but works. Fastest way I found to parse out the dates into separate fields.
update ride set StartMonth = '1' where StartDate like '1/%/%';
update ride set StartMonth = '2' where StartDate like '2/%/%';
update ride set StartMonth = '3' where StartDate like '3/%/%';
update ride set StartMonth = '4' where StartDate like '4/%/%';
update ride set StartMonth = '5' where StartDate like '5/%/%';
update ride set StartMonth = '6' where StartDate like '6/%/%';
update ride set StartMonth = '7' where StartDate like '7/%/%';
update ride set StartMonth = '8' where StartDate like '8/%/%';
update ride set StartMonth = '9' where StartDate like '9/%/%';
update ride set StartMonth = '10' where StartDate like '10/%/%';
update ride set StartMonth = '11' where StartDate like '11/%/%';
update ride set StartMonth = '12' where StartDate like '12/%/%';

update ride set StartYear = '2010' where StartDate like '%/2010 %';
update ride set StartYear = '2011' where StartDate like '%/2011 %';
update ride set StartYear = '2012' where StartDate like '%/2012 %';

create index ind_startYear on ride(StartYear);
create index ind_startMonth on ride(StartMonth);

drop table if exists Outgoing;
create table Outgoing (station, count);

drop table if exists Incoming;
create table Incoming (station, count);

-- Create aggregate tables
insert into outgoing select distinct StartStation, count(*) from ride group by StartStation order by startStation;
insert into Incoming select distinct EndStation, count(*) from ride group by EndStation order by EndStation;
```
