---
author: jokecamp
comments: true
date: 2012-03-16 18:52:28+00:00
layout: post
slug: make-your-sqlite-bulk-inserts-very-fast-in-c
title: Make your SQLite bulk inserts very fast in C# .NET
desc: 'Learn how to increase performance with performing bulk SQLite inserts in C# .NET'
wordpress_id: 158
categories:
- Code
tags:
- .NET
- C#
- sqlite
---

In SQLite if you insert one record at a time then they are wrapped in individual transactions. In order to for your bulk inserts to perform very fast then you need to wrap them all in a single transaction. See [SQLite FAQ](http://www.sqlite.org/faq.html#q19). You should be able to able to insert very quickly without having to worry about PRAGMAs

I was able to insert 1 million rows in about 4 seconds.

I completed this example with a c# console application in Visual Studio 2010. I installed the NuGet package called System.Data.SQLite (x86) version 1.0.79.0. This will add a project reference to System.Data.SQLite (and System.Data.SQLite.Linq which is not needed for this example). The NuGet package is "The official SQLite database engine combined with a complete ADO.NET provider all rolled into a single mixed-mode assembly for x86."

Create your Person Table in SQLite for our test:

```sql
CREATE TABLE IF NOT EXISTS Person (FirstName TEXT, LastName TEXT);
```

Don't forget your using statement: using System.Data.SQLite;

```csharp
// Creates new sqlite database if it is not found
using (var conn = new SQLiteConnection(
    @"Data Source=C:\Projects\sqlite\test.sqlite"))
{
  // Be sure you already created the Person Table!

  conn.Open();

  var stopwatch = new Stopwatch();
  stopwatch.Start();

  using (var cmd = new SQLiteCommand(conn))
  {
    using (var transaction = conn.BeginTransaction())
    {
        // 100,000 inserts
        for (var i = 0; i < 1000000; i++)
        {
            cmd.CommandText =
                "INSERT INTO Person (FirstName, LastName) VALUES ('John', 'Doe');";
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }
  }

  Console.WriteLine("{0} seconds with one transaction.",
    stopwatch.Elapsed.TotalSeconds);

  conn.Close();
}
```
