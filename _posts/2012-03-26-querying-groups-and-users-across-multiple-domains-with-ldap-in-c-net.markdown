---
author: jokecamp
comments: true
date: 2012-03-26 17:11:20+00:00
layout: post
slug: querying-groups-and-users-across-multiple-domains-with-ldap-in-c-net
title: Querying Groups and Users across multiple domains with LDAP in C# .NET
desc: 'Example C# .NET code to query groups and users across multiple domains with LDAP'
wordpress_id: 193
categories:
- Code
tags:
- .NET
- ActiveDirectory
- C#
- LDAP
---

I recently needed to fix some LDAP queries using DirectoryEntry and DirectorySearcher. The query was very simple. Find a group and return all the members of that group. There were two problems with the existing C# code:

  1. the group DN (distinguished name) was hard coded
  2. the groups and users were on different domains

**Solution to #1**

The group should have never been hard coded (even if it is in a config file.) The problem is that once a group moves the query will no longer work. A much better approach is to query first for the group by the exact name and return the full distinguished name (DN). Then use the group DN to get all the members of the group in a separate query.

**Solution to #2**

The network I was looking at involved two different domains. My users were on one domain but the groups were on another. I had no problems querying for the group. However, when I tried to get the group members that resided on a different domain the Directory Searcher returned zero members.

I found two different ways to solve this and implemented both in the end.

  - Query the **Global catalog (GC)** instead of a specific LDAP domain (assuming the GC has been setup correctly).
	- Set the **[System.DirectoryServices.ReferralChasingOption ReferralChasing](http://msdn.microsoft.com/en-us/library/system.directoryservices.referralchasingoption.aspx) option to ALL**.

All together an example for ad.example.org

```csharp
using (var de = new DirectoryEntry("GC://dc=ad,dc=example,dc=org"))
{
    using (var ds = new DirectorySearcher(de))
    {
        ds.ReferralChasing = ReferralChasingOption.All;
        // perform search
    }
}
```

I've included my code for my custom LDAP Provider classes and implementation. It only performs the group and member lookups. Please feel free to use it and critique it.

Calling from the main entry point:

```csharp
var provider = new LdapProvider("GC://dc=ad,dc=example,dc=org");
var members = provider.GetMembersOfGroup("DevelopmentTeam");

foreach (var m in members)
{
    if (!m.IsAccountDisabled)
        Console.WriteLine("{0} {1} {2} {3}", m.FirstName, m.LastName, m.SamAccountName, m.UserAccountControl);
}
```

Below is the Provider Implementation. I reuse my DirectoryEntry instance to reduce the number of queries and improve performance.

```csharp
public class LdapProvider
{
    // Called LDAP Path but can handle GC paths too
    private readonly string _ldapPath;

    public LdapProvider(string path)
    {
        _ldapPath = path;
    }

    /// <summary>
    /// Provide the friendly name of the group to get all members of the group.
    /// </summary>
    /// <param name="groupName"></param>
    /// <returns></returns>
    public IEnumerable<LdapUserData> GetMembersOfGroup(string groupName)
    {
        var members = new List<LdapUserData>();

        try
        {
            using (var directoryEntry = new DirectoryEntry(_ldapPath))
            {
                var groupDistinguishedName = GetGroupDistinguishedName(directoryEntry, groupName);
                members = GetMembersOfGroup(directoryEntry, groupDistinguishedName).ToList();
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine("Failed to query LDAP users {0} on {1}. {2}", groupName, _ldapPath, ex.Message);
        }

        return members;
    }

    private void SetupDefaultPropertiesOnDirectorySearcher(DirectorySearcher searcher)
    {
        // allow us to use references to other active dir domains.
        searcher.ReferralChasing = ReferralChasingOption.All;
    }

    private string GetGroupDistinguishedName(DirectoryEntry directoryEntry, string groupName)
    {
        var distinguishedName = "";

        var filter = string.Format("(&(objectClass=group)(name={0}))", groupName);
        var propertiesToLoad = new string[] {"distinguishedName"};

        using (var ds = new DirectorySearcher(directoryEntry, filter, propertiesToLoad))
        {
            SetupDefaultPropertiesOnDirectorySearcher(ds);

            var result = ds.FindOne();
            if (result != null)
            {
                distinguishedName = result.Properties["distinguishedName"][0].ToString();
            }
        }

        return distinguishedName;
    }

    private IEnumerable<LdapUserData> GetMembersOfGroup(DirectoryEntry directoryEntry, string groupDistinguishedName)
    {
        var members = new List<LdapUserData>();

        if (string.IsNullOrEmpty(groupDistinguishedName))
        {
            throw new Exception("Group name not provided. Cannot look for group members.");
        }

        var filter = string.Format("(&(objectClass=user)(memberof={0}))", groupDistinguishedName);

        // Only load what we need
        var propertiesToLoad = new string[] {"givenname", "samaccountname", "sn", "useraccountcontrol"};

        using (var ds = new DirectorySearcher(directoryEntry, filter, propertiesToLoad))
        {
            SetupDefaultPropertiesOnDirectorySearcher(ds);

            // get all members in a group
            foreach (SearchResult result in ds.FindAll())
            {
                try
                {
                    members.Add(new LdapUserData()
                                    {
                                        SamAccountName = result.Properties["samaccountname"][0].ToString(),
                                        UserAccountControl =
                                            (result.Properties["useraccountcontrol"][0] is int)
                                                ? (int) result.Properties["useraccountcontrol"][0]
                                                : 0,
                                        FirstName = result.Properties["givenname"][0].ToString(),
                                        LastName = result.Properties["sn"][0].ToString()
                                    });
                }
                catch (Exception)
                {
                    Console.WriteLine("Failed to add user.");
                }
            }
        }

        return members;
    }
}

/// <summary>
/// Very lightweight class to hold user account data
/// </summary>
public class LdapUserData
{
    /// <summary>
    /// SAM = Security Accounts Manager
    /// </summary>
    public string SamAccountName { get; set; }

    /// <summary>
    /// Bit field flags that control the behavior of the AD user account.
    ///
    /// A few relevant ones:
    /// 2   = ACCOUNTDISABLE
    /// 512 = NORMAL_ACCOUNT
    ///
    /// 514 = NORMAL_ACCOUNT && ACCOUNTDISABLE
    /// </summary>
    public int UserAccountControl { get; set; }

    /// <summary>
    /// Check bit flag and include anything without a valid name as disabled.
    /// </summary>
    public bool IsAccountDisabled
    {
        get { return UserAccountControl == 514 || string.IsNullOrEmpty(SamAccountName); }
    }

    public string FirstName { get; set; }
    public string LastName { get; set; }

}
```
