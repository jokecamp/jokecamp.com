---
author: Joe Kampschmidt
comments: true
layout: post
slug: running-local-mongodb-with-npm-scripts
title: Running a local MongoDB with npm scripts for dev work
desc: 'Simple windows and osx npm scripts to run a local MongoDB database for development work.'
tags:
- mongodb
- npm
---

Below are two very simple [npm scripts](https://docs.npmjs.com/misc/scripts) to run a local MongoDb instance for development work. There are two scripts that will work for Windows (x64) environments and mac OS X environments.

#### 1) First install MongoDb

  - For OS X I prefer `brew install mongodb`
  - On Windows x64 install the [official MSI](https://www.mongodb.org/downloads#production)
   - You may want to add to the path to your environment variable: `set PATH=%PATH%;C:/Program Files/MongoDB/Server/3.2/bin/`

#### 2) Add the following to your `packages.json` file under `scripts`

```
"mongo-mac": "mkdir mongo-db; mongod --dbpath mongo-db",
"mongo-win": "md mongo-db & \"C:/Program Files/MongoDb/Server/3.2/bin/mongod.exe\" --dbpath mongo-db"
```

#### 3) Now windows users can easily run with `npm run mongo-win` and OS X users can use `npm run mongo-mac`.

#### Gotchas

- Your mongod.exe path may be slightly different.
- The script assumes your database files will go in project folder `mongo-db`. I added it to `.gitignore.`
- You must keep your console open to run the db instance. I prefer this to always running the service so I can easily close everything when done.
