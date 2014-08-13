---
author: jokecamp
comments: true
date: 2013-06-11 17:03:59+00:00
layout: post
slug: using-postsharp-to-add-servicestack-route-attributes
title: Using PostSharp to Add ServiceStack Route Attributes
desc: "Using PostSharp to add custom ServiceStack route attributes to your DTOs"
wordpress_id: 853
categories:
- Code
tags:
- api
- restful
- servicestack
---

**Updated**:  The latest version of ServiceStack (v3.9.53) now includes the [.ext routing option](https://github.com/ServiceStack/ServiceStack/wiki/Routing#content-negotiation) using the AllowRouteContentTypeExtensions configuration property. See The comments section for more information. I would use the framework instead of this custom approach. This example now really serves as a demonstration of using PostSharp to apply attributes in general.

* * *

**An aspect oriented approach to adding data format to the route**

A common approach in restful APIs is to allow the consumer to provide the content type in the route as a file extension. For example: /api/movies.json or /api/movies.xml?genre=Action.

Currently, ServiceStack supports different formats via the querystring "format" parameter or via the headers. You can easily override the HTTP content type by simply specifying urls like the following: /api/movies?format=json. One way to make ServiceStack work with the file extensions is to explicitly put the formats into the RouteAttribute paths and adding your own RequestFilter to check for a provided format. See the RequestFilter code at the bottom of the post.

For Example:

```csharp
  [Routes("/shows.json")]
  [Routes("/shows.xml")]
  [Routes("/shows.csv")]
  [Routes("/shows.jsv")
  [Routes("/shows.jsv")
  [Routes("/shows")
```

However, this becomes cumbersome to manage. If you are using Aspect oriented programming and PostSharp there is an easier way. You can create an aspect to decorate your DTOs with the accepted content types.

**PostSharp Attribute to Turn one Route attribute into many route attributes.**

```csharp
///
/// Turns one Route into many routes with different data formats
///
/// For example:
///
///     [RoutesWithDataFormats("/shows") => [Routes("/shows.json")]
///                                         [Routes("/shows.xml")]
///                                         [Routes("/shows.csv")]
///                                         [Routes("/shows.jsv")]
///                                         [Routes("/shows")]
///
[MulticastAttributeUsage(MulticastTargets.Class, Inheritance = MulticastInheritance.Strict)]
[Serializable]
public sealed class RoutesWithDataFormatsAttribute : TypeLevelAspect, IAspectProvider
{
    private string _path;
    private string _verbs;

    public RoutesWithDataFormatsAttribute(string path, string verbs)
    {
        _path = path;
        _verbs = verbs;
    }

    public RoutesWithDataFormatsAttribute(string path)
    {
        _path = path;
    }

    // This method is called at build time and should just provide other aspects.
    public IEnumerable ProvideAspects(object targetElement)
    {
        var attributes = new List();

        var formats = new[] {"json", "xml", "csv", "jsv"};

        // Add data format routes first!
        foreach (var format in formats)
        {
            var dataFormatPath = string.Format("{0}.{1}", _path, format);
            attributes.Add(CreateAspect(targetElement, dataFormatPath, _verbs));
        }

        // Then add plain route last
        attributes.Add(CreateAspect(targetElement, _path, _verbs));

        return attributes;
    }

    private AspectInstance CreateAspect(object targetElement, string path, string verbs)
    {
        var constructor = string.IsNullOrWhiteSpace(verbs)
                        ? new ObjectConstruction(typeof(RouteAttribute), path)
                        : new ObjectConstruction(typeof(RouteAttribute), path, verbs);

        var introduceDataContractAspect = new CustomAttributeIntroductionAspect(constructor);

        return new AspectInstance(targetElement, introduceDataContractAspect);
    }
}
```

**AppHost Request Filters to make it work**

You will of course need to filter for these formats and override the content types. You can do this by adding a RequestFilter in your AppHost.

```csharp
// Filters happen before every request! Even before auth check
this.RequestFilters.Add((httpReq, httpResp, requestDto) =>
    {
        // allow .xml and .json
        if (httpReq.PathInfo.EndsWith(".xml"))
        {
            httpReq.ResponseContentType = ContentType.Xml;
        }
        else if (httpReq.PathInfo.EndsWith(".json"))
        {
            httpReq.ResponseContentType = ContentType.Json;
            httpResp.ContentType = ContentType.Json;
        }
        // and ect for the other data types. You can refactor this into cleaner code
    });
```
