---
author: Joe Kampschmidt
comments: true
date: 2011-11-10 21:46:46+00:00
layout: post
slug: create-customvalidator-to-enforce-maxlength-on-a-multiline-textbox
title: Create CustomValidator to enforce MaxLength on a MultiLine TextBox
wordpress_id: 23
categories:
- Code
tags:
- .NET
- C#
- UI
- Validation
---

[ASP .NET TextBoxes ](http://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.textbox.maxlength.aspx) with mode = MultiLine do not support the MaxLength property. MultiLine TextBoxes are rendered as html textareas and this property is not supported in many browsers. (I believe HTML5 will be supporting it)

**Solution**  

Create a custom validator and a provide your own ServerValidaton method. Set the MaxLength property of your textbox, the ControlToValidate and set the custom validator property to OnServerValidate="ValidateMaxLength"

HTML

```html
<asp:TextBox ID="uxTextBox" runat="server"
                            TextMode="MultiLine"
                            MaxLength="2" />

<asp:CustomValidator ID="uxValidator"
                        runat="server"
                        ControlToValidate="uxTextBox"
                        OnServerValidate="ValidateMaxLength"
                        ErrorMessage="Too many characters!" />

<asp:Button ID="uxButton" runat="server" Text="Count" />
```

C#

```csharp
protected void ValidateMaxLength(object sender, ServerValidateEventArgs e)
{
    e.IsValid = true;

    var validator = (sender as CustomValidator);
    if (validator != null)
    {
        // validator and textbox need to be in the same
        // NamingContainer for FindControl to work
        var textbox = validator.FindControl(validator.ControlToValidate);
        if (textbox != null && textbox is TextBox)
        {
            e.IsValid = (textbox as TextBox).MaxLength >= e.Value.Length;
        }
    }
}
```
