---
layout: post
slug: getting-started-with-openfootball-data
title: Getting started with openfootball data
desc: 'A quick tutorial on using the openfootball data sets'
tags:
- data
---

mkdir openfootball
cd openfootball

1) Pick a data set repository. We will use [https://github.com/openfootball/eng-england](https://github.com/openfootball/eng-england)

2) install sportdb

`gem install sportdb`

```
$ gem list sportdb

*** LOCAL GEMS ***

sportdb (1.11.0)
sportdb-models (1.14.0)
sportdb-service (0.4.0)
sportdb-update (0.1.1)
```

What is [ruby gem](http://guides.rubygems.org/what-is-a-gem/)?

sportdb new en


# Understanding the OpenFootball Text Format (Mini Language)

https://github.com/sportdb/sport.db.models/blob/b3245d858fb89757eb8e91e1da8ccacd001db9c0/lib/sportdb/readers/game.rb

Event Keys:
  league
  season
  start_at / begin_at (%Y-%m-%d) date
  end_at / stop_at date
  grounds, stadiums, venues array
  teams array
  fixtures / sources

Goals format
  [Neymar 12']

https://github.com/sportdb/sport.db.models/blob/b3245d858fb89757eb8e91e1da8ccacd001db9c0/lib/sportdb/readers/game.rb
