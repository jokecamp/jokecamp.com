---
author: jokecamp
comments: true
date: 2012-04-06 18:39:04+00:00
layout: post
slug: simplest-code-needed-to-create-an-excel-2010-file-with-open-xml-sdk-2-0
title: Simplest code needed to create an Excel 2010 file with Open XML SDK 2.0
desc: 'The bare minimum code needed to create an Excel 2010 file with Open XML SDK 2.0 in C#'
wordpress_id: 233
categories:
- Code
tags:
- .NET
- C#
- OpenXML
---

Open XML can be very frustrating. In order to help myself understand the package, parts and relationships I have tried to find the **bare minimum** amount of C# code needed to generate a **valid empty excel file**. The inline constructors help to show the hierarchy.

Please let me know if you have found a simpler and less verbose way.

**The bare minimum code required:**

```csharp
// By default AutoSave will be true and Editable = true, and Type = xlsx
using (var doc = SpreadsheetDocument.Create("Empty.xlsx", SpreadsheetDocumentType.Workbook))
{
  // Creates 4 things: WorkBookPart, WorkSheetPart, WorkSheet, SheetData
  doc.AddWorkbookPart().AddNewPart<WorksheetPart>().Worksheet = new Worksheet(new SheetData());

  doc.WorkbookPart.Workbook =
    new Workbook(
      new Sheets(
        new Sheet
          {
            // Id is used to create Sheet to WorksheetPart relationship
            Id = doc.WorkbookPart.GetIdOfPart(doc.WorkbookPart.WorksheetParts.First()),
            // SheetId and Name are both required
            SheetId = 1,
            Name = "Empty Sheet"
          }));
}
```

This will create a file called Empty.xlsx in the directory you ran the code from. If you wanted to work from this code you could start by injecting your own data into the SheetData() constructor.

Other Open XML sources:

  - [MSDN Blog: Creating an Excel spreadsheet from scratch using OpenXML](http://blogs.msdn.com/b/chrisquon/archive/2009/07/22/creating-an-excel-spreadsheet-from-scratch-using-openxml.aspx)
  - [OpenXML SDK 2.0: Export a DataTable to Exc](http://www.lateral8.com/articles/2010/3/5/openxml-sdk-20-export-a-datatable-to-excel.aspx)
  - [MSDN: Open XML Cell Class](http://msdn.microsoft.com/en-us/library/documentformat.openxml.spreadsheet.cell.aspx)
  - [MSDN: How to: Create a Spreadsheet Document by Providing a Filename](http://msdn.microsoft.com/en-us/library/ff478153.aspx)
