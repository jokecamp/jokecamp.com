---
author: jokecamp
comments: true
date: 2013-09-07 15:51:13+00:00
layout: post
slug: code-generation-using-servicestack-ormlite-and-t4-text-templates
title: Code Generation using ServiceStack.OrmLite and T4 Text templates
desc: "How to generate code using ServiceStack OrmLite and Visual Studio T4 text templates to reduce writing boilerplate code."
wordpress_id: 1038
categories:
- Code
tags:
- C#
- codegeneration
- OrmLite
- servicestack
- sql
---

I was reading the [ServiceStack.OrmLite](https://github.com/ServiceStack/ServiceStack.OrmLite) documentation on GitHub and stumbled upon an often overlooked T4 templates located in the /src/t4 folder.

From the page:

 > [Guru Kathiresan](https://github.com/gkathire) continues to enhance OrmLite's T4 Template support which are useful when you want to automatically generate POCO's and strong-typed wrappers for executing stored procedures.

Some of the key benefits:

  - **generate stored procedure code for every user stored procedure in your database**
  - **generate all your DTOs (called POCOs in template) based on every user table in your database**
	 - Alias attribute for primary Keys created as Id allowing you to harness the benefits of the IHasId interface
   - AutoIncrement attribute for primary keys
   - Required attribute for non null database columns
  - very easy to customize the templates to your organization's standards/conventions

### **When and why use the T4 templates?**

If you are using code first development then you won’t need the templates. However, most developers don’t get to start from scratch on projects. The templates become crucial when working with a large pre-existing database and code base.

We are all tired of writing CRUD operations. There is no need to be typing out DTO definitions and writing stored procedure execution code manually. In most cases though you won't be able to just cleanly swap in the OrmLite code. You might have logic in existing stored procedures that refactoring would be very error prone. In these cases I’ve found that a combination of OrmLite CRUD operations and the more rudimentary System.Data.IDbCommand code is an ideal solution. This works very well because OrmLite works as simple extensions on the System.Data.* classes. In a recent project of mine I have a base OrmLite repo and have the ability to override any of the CRUD operations to use stored procedures that have been generated from the T4 templates as needed.

### **Improvements to the T4 templates**

I’ve added some more features to the templates to a fork of the project. These new templates can be found at GitHub [jokecamp/ServiceStack.OrmLite/src/T4](https://github.com/jokecamp/ServiceStack.OrmLite/tree/master/src/T4). You will want to grab all three files (OrmLite.Core.ttinclude, OrmLite.Poco.tt, OrmLite.SP.tt) drop them into a class library and make sure you add your database connection string to the web.config. The main changes add support for **databases with multiple schemas** and some code changes to **provide support for output parameters**.

My changes include:

  * Replace enum SPParamDir with System.Data.ParameterDirection
  * Added Schema to table definition queries
  * Added ordering to SQL meta data queries by Schema then by object name for DTOs and Stored Procedures
  * Added [Schema("")] attribute to generated DTOs to support multiple schemas
  * Changed stored procedure method names to use “schema_storedProcedureName”
  * Created wrapper class called OrmLiteStoredProcedureWrapper

I created the [OrmLiteStoredProcedureWrapper](https://github.com/jokecamp/ServiceStack.OrmLite/blob/master/src/T4/OrmLite.SP.tt#L33) class for accessing output parameters. After the execution of the stored procedure you will need access to the DbCommand parameters to get any output parameters. Currently the OrmLiteSPStatement class does not expose the private DbCommand property. For now I just created a wrapper class to return the statement and command so that developers can just drop in the changes without changing the core OrmLite library. However, another solution would be to expose the DbCommand property as public in the base OrmLite project.

Here is an example of the stored procedure generated code:


```csharp
public static OrmLiteStoredProcedureWrapper dbo_USP_SavePerson(
    this IDbConnection dbConnection,
    int? personID = null, DateTime? returnTime = null, int? returnID = null)
{
    var dbCommand = (DbCommand)dbConnection.CreateCommand();
    dbCommand.CommandText = "dbo.USP_SavePerson";
    dbCommand.CommandType = CommandType.StoredProcedure;
    dbCommand.Transaction = OrmLiteConfig.TSTransaction != null ?
	  (DbTransaction)OrmLiteConfig.TSTransaction : null;

    dbCommand.Parameters.Add(CreateNewParameter(dbCommand, "personID", personID,
	  ParameterDirection.Input, DbType.Int32));

    dbCommand.Parameters.Add(CreateNewParameter(dbCommand, "ReturnTime", returnTime,
	  ParameterDirection.InputOutput, DbType.DateTime));

    dbCommand.Parameters.Add(CreateNewParameter(dbCommand, "ReturnID", returnID,
	  ParameterDirection.InputOutput, DbType.Int32));

    return new OrmLiteStoredProcedureWrapper
    {
        Statement = new OrmLiteSPStatement(dbCommand),
        DbCommand = dbCommand
    };
}
```


In the end your stored procedure execution and retrieval of the output parameters can look like the following:

```csharp
using (var db = _connectionFactory.OpenDbConnection())
{
    ReturnTime = DateTime.Now;
    var sp = db.dbo_USP_SavePerson(PersonID, ReturnTime, ReturnId);

    var rowsAffected = sp.Statement.ExecuteNonQuery();
    // get output parameters
    PersonID = ReturnId = Convert.ToInt32(sp.DbCommand.Parameters["ReturnID"].Value);
    ReturnTime = Convert.ToDateTime(sp.DbCommand.Parameters["ReturnTime"].Value);
}
```

**Special thanks to [Guru Kathiresan](https://github.com/gkathire) for creating the templates.**

_At the end of the day the less code we manually write the better._
