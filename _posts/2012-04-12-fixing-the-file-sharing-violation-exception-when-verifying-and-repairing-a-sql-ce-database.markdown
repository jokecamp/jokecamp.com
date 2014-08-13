---
author: jokecamp
comments: true
date: 2012-04-12 00:45:27+00:00
layout: post
slug: fixing-the-file-sharing-violation-exception-when-verifying-and-repairing-a-sql-ce-database
title: 'Fixing the File sharing violation Exception when Verifying and Repairing a SQL CE database'
desc: 'Fixing the File sharing violation Exception when Verifying and Repairing a SQL CE database'
wordpress_id: 266
categories:
- Code
tags:
- .NET
- C#
- db
- sqlce
---

**The Problem:**

In C# .NET If you try to Verify a SQL Server Compact Edition (SQL CE) database file using the [SqlCeEngine.Verify](http://msdn.microsoft.com/en-us/library/system.data.sqlserverce.sqlceengine.aspx) method while the database has a different connection already open you will receive false from the Verify method. If you try to use SqlCeEngine.Repair then you will receive the following SqlCeException **There is a file sharing violation. A different process might be using the file.** This is because the **Verify and Repair require exclusive access to the physical file.**

One easy way to replicate the SqlCeException file sharing violation is to open your sdf file in SQL Server Management Studio and run a simple query and keep the query window open (this keeps the connection open). Then try to verify and repair the sdf file via your code and you will receive the exception.

Another way to replicate the sharing violation exception is to use two threads in your code. One to keep a connection open and another to repair the file. The code below will show you how to do this but it also shows you a possible solution. In order to get the exception change the Engine constructors to use the conn variable instead of the connBackup.

**Solution:**

In most cases this usually isnt' a problem since your application is managing the connections. In my case though that was not a safe assumption. I needed the code to assume there could be un-managed connections that I needed to work around.

There is a little more information on this forum about the issue [MSDN: SqlCeEngine - Repair / Verify - General database best practise](http://social.msdn.microsoft.com/Forums/en-US/sqlce/thread/11197c0d-312a-4362-b3a7-0e4619b365c1). However, there is not much actual code on a possible solution.

One solution is to make a copy of your actual sdf database. Run the verify and repair against that copy. If the database can be verified we can just delete the copy and proceed happily. If the database if not valid but we can repair the copy then we can swap the repaired copy for the actual database file. If we cannot repair the copy there is not much we can do.

This works for me due to the very small size of the database file. I am unsure how this would work we large files. (Your SQLCE files should not be too big anyways though, right?)

The below code shows the make a copy and swap solution.

```csharp
static void Main(string[] args)
{
    var file = @"C:\Projects\Test.sdf";
    var fileBackup = @"C:\Projects\TestBackup.sdf";

    var conn = string.Format("Data Source={0};", file);
    var connBackup = string.Format("Data Source={0};", fileBackup);

    // Creates an empty database to test with
    if (!File.Exists(file))
    {
        using (var engine = new SqlCeEngine(conn))
        {
            Console.WriteLine("Creating a test Database " + conn);
            engine.CreateDatabase();
        }
    }

    // Create a new thread and keep the connection open to conflict
    // with the verify/repair connection in the main thread
    var thread = new Thread(
        () =>
        {
            using (var connection = new SqlCeConnection(conn))
            {
                // Keeping a connection open for 30 seconds
                Console.WriteLine("Connection Opened");
                connection.Open();
                Thread.Sleep(30000);
                connection.Close();
                Console.WriteLine("Connection Closed");
            }
        }

        );
    thread.Start();

    // Pause a bit to make sure prev conn opens first
    Thread.Sleep(500);

    // Create second copy to verify and repair
    File.Copy(file, fileBackup, true);

    using (var engine = new SqlCeEngine(connBackup))
    {
        if (engine.Verify())
        {
            Console.WriteLine("File was successfully verified. Great!");
        }
        else
        {
            engine.Repair(null, RepairOption.RecoverAllPossibleRows);

            // Did the repair work?
            if (engine.Verify())
            {
                Console.WriteLine("Backup file has been repaired. Swap the repaired with the actual.");
                File.Copy(fileBackup, file, true);
            }
        }
    }
    // cleanup backup file
    File.Delete(fileBackup);
}
```
