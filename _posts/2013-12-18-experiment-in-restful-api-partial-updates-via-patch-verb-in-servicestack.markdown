---
author: jokecamp
comments: true
date: 2013-12-18 01:00:48+00:00
layout: post
slug: experiment-in-restful-api-partial-updates-via-patch-verb-in-servicestack
title: Experiment in Restful API Partial Updates via PATCH verb in ServiceStack
desc: "A code experiment and demo of a ServiceStack API implementation of a partial resource update with the PATCH verb."
wordpress_id: 1238
categories:
- Code
tags:
- api
- C#
- PATCH
- restful
- servicestack
---

I've recently been thinking about how to implement partial updates in a restful API. Partial updates can be achieved in ServiceStack with very little code using OrmLite. See this old google groups post [Partial Updates via HTTP Patch (or X-HTTP-Method-Override:PATCH)](https://groups.google.com/forum/#!topic/servicestack/H9lov15rR14) for my starting point.

### Restful Implementations

We usually see two different approaches in restful APIs when it comes to partial updates.

  1. **Use the PATCH verb on a single resource**. Not all browsers play nice with PATCH. You may need to use the `X-HTTP-Method-Override:PATCH` header. I will be using this approach.
  2. **Use the POST verb on a single resource**. In this alternative setup the HTTP methods typically perform the following:
    * POST to list resource for inserts.
    * PUT to single resource for updates.
    * POST to single resource with fields you want for updates.

This could also work but I feel like it is not as clear to the consumers. The PUT vs POST argument does not seem to be over yet.

### Basic Implementation via PATCH

Using the first approach, PATCH method my code will operate via the following

  * The consumer must provide the fields they wish to update in the querystring as a comma separated value
  * The consumer will request a PATCH to /resource/{id} (/Leagues/1 in our example code)
  * The Service PATCH method will build the SQL by using reflection and OrmLite. We can utilize the built in ModelDefinition and use the OrmLite UpdateOnly method.

Before looking at the code, a couple notes:

  * The use of the querystring to provide a ?fields=Name,Abbreviation allows us to use the common practice in ServiceStack of passing the entire DTO. Our JSON can   contain all the fields but the service will only update the fields provided in the querystring. It would be great to remove this condition and update all fields that are provided in the JSON. I would love to see someone adapt this code and get that working. However, for this experiment it was far simpler to use the querystring.
  * You are able to PATCH partial JSON DTOs as long as ServiceStack succeeds in deserializing the JSON into your DTO.
  * I've only tested this with simple DTOs. It would need to be tested and modified to work with more complex DTOs and DTO properties.
  * The code was written with ServiceStack v4 but can easily be adapted to v3.

Clone and run the example yourself on Github via [jokecamp/ServiceStackv4-Demo-TeamsApi](https://github.com/jokecamp/ServiceStackv4-Demo-TeamsApi/blob/master/LeagueService.cs) or just view the important code below.

Below are the League DTO and League Service:

```csharp
[Route("/leagues/{Id}")]
public class League
{
    [AutoIncrement, PrimaryKey]
    public int Id { get; set; }
    public string Name { get; set; }
    public string Abbreviation { get; set; }

    public DateTime DateCreated { get; set; }
    public DateTime DateUpdated { get; set; }
}

public class LeagueService : Service
{
    public object Patch(League request)
    {
        var fields = Request.QueryString["fields"].Split(new[] { ',' });

        if (!fields.Any())
            throw new Exception("Provide the fields to update via ?fields= in querystring");

        Db.UpdateOnly(request, delegate(SqlExpression expression)
        {
            foreach (var field in fields)
            {
                var match = ModelDefinition.Definition
                    .FieldDefinitions
                    .FirstOrDefault(x => x.FieldName.ToLower() == field.ToLower());
                if (match != null)
                    expression.UpdateFields.Add(match.FieldName);
            }

            return expression.Where(x => x.Id == request.Id);
        });

        // returning entire object. You may want to do a different response code
        return Db.SingleById(request.Id);
    }

    // omitting the other HTTP verbs methods. use your imagination.
}
```

Example Integration Test

```csharp
[Test]
public void CanPerform_PartialUpdate()
{
    var client = new JsonServiceClient("http://localhost:53990/api/");

    // back date for readability
    var created = DateTime.Now.AddHours(-2);

    // Create a record so we can patch it
    var league = new League() {Name = "BEFORE", Abbreviation = "BEFORE", DateUpdated = created, DateCreated = created};
    var newLeague = client.Post(league);

    // Update Name and DateUpdated fields. Notice I don't want to update DateCreatedField.
    // I also added a fake field to show it does not cause any errors
    var updated = DateTime.Now;
    newLeague.Name = "AFTER";
    newLeague.Abbreviation = "AFTER"; // setting to after but it should not get updated
    newLeague.DateUpdated = updated;

    client.Patch("http://localhost:53990/api/leagues/" + newLeague.Id + "?fields=Name,DateUpdated,thisFieldDoesNotExist", newLeague);

    var updatedLeague = client.Get(newLeague);

    Assert.AreEqual(updatedLeague.Name, "AFTER");
    Assert.AreEqual(updatedLeague.Abbreviation, "BEFORE");
    Assert.AreEqual(updatedLeague.DateUpdated.ToString(), updated.ToString(), "update fields don't match");
    Assert.AreEqual(updatedLeague.DateCreated.ToString(), created.ToString(), "created fields don't match");

    // double check
    Assert.AreNotEqual(updatedLeague.DateCreated, updatedLeague.DateUpdated);
}
```
