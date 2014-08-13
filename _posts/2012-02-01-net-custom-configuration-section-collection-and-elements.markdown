---
author: jokecamp
comments: true
date: 2012-02-01 20:29:44+00:00
layout: post
slug: net-custom-configuration-section-collection-and-elements
title: .NET Custom Configuration Section, Collection and Elements
desc: 'Example code for creating custom C# .NET collections and elements for use in a configuration file.'
wordpress_id: 55
categories:
- Code
tags:
- .NET
- .net configuration
- configuration
---

This site about [writing custom configurationsection](http://www.abhisheksur.com/2011/09/writing-custom-configurationsection-to.html) offers the most straight forward solution. Make sure you implement all methods the author describes for your version of the ConfigurationElementCollection class. I tried just the ones the abstract class requires but could not get it to work. I've posted my own implementation using the authors guidelines to create a configuration collection of jobs.

Another useful link from a [stackoverflow question](http://stackoverflow.com/questions/2568454/code-required-to-use-foreach-on-my-own-custom-appsettings).

**App.Config**

```xml
<?xml version="1.0"?>
<configuration>
  <configSections>
    <section name="jobSection"
      type="MyConfiguration.JobSection, MyConfiguration" />
  </configSections>
  <jobSection>
    <jobs>
      <job id="1" name="Job Name A" />
      <job id="2" name="Job Name B" />
    </jobs>
  </jobSection>
</configuration>
```

**My C# classes**

```csharp
namespace MyConfiguration
{
    public class JobElement : ConfigurationElement
    {
        [ConfigurationProperty("id", IsRequired = true, IsKey = true)]
        public int Id
        {
            get { return (int)base["id"]; }
        }

        [ConfigurationProperty("name", IsRequired = false)]
        public string Name
        {
            get { return (string)base["name"]; }
        }
    }

    [ConfigurationCollection(typeof(JobElement))]
    public class JobElementCollection : ConfigurationElementCollection
    {
        internal const string PropertyName = "job";

        public override ConfigurationElementCollectionType CollectionType
        {
            get
            {
                return ConfigurationElementCollectionType.BasicMapAlternate;
            }
        }
        protected override string ElementName
        {
            get
            {
                return PropertyName;
            }
        }

        protected override bool IsElementName(string elementName)
        {
            return elementName.Equals(PropertyName,
              StringComparison.InvariantCultureIgnoreCase);
        }

        public override bool IsReadOnly()
        {
            return false;
        }

        protected override ConfigurationElement CreateNewElement()
        {
            return new JobElement();
        }

        protected override object GetElementKey(ConfigurationElement element)
        {
            return ((JobElement)(element)).Id;
        }

        public JobElement this[int idx]
        {
            get { return (JobElement)BaseGet(idx); }
        }
    }

    public class JobSection : ConfigurationSection
    {
        [ConfigurationProperty("jobs")]
        public JobElementCollection Jobs
        {
            get { return ((JobElementCollection)(base["jobs"])); }
            set { base["jobs"] = value; }
        }
    }
}
```

**C# Code to reference config**

```csharp
var section = ConfigurationManager.GetSection("jobSection");
if (section != null)
{
    var jobs = (section as JobSection).Jobs;
    Console.WriteLine(jobs.Count);
    for (int i = 0; i < jobs.Count; i++)
    {
        Console.WriteLine("{0} - {1}", jobs[i].Id, jobs[i].Name);
    }
}
```
