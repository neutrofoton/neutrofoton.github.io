---
layout: post
title: "ASP.NET Session State in Database"
date: 2013-07-22 15:12:34 +0800
comments: true
categories: [csharp, aspnet]
---
At some point we need to store information when dealing with web application.
Storing information in web application can be implemented in some ways such as session and cookies.

Dealing with session case, previously I never got a situation where I should store the session state in persistance way till I got a condition implementing this way.

The scenario is we want to implement load balancer to our applications and services (web service, wcf) that hosted in IIS. The aim of this enhancement basically to improve availability and scalability of existing in house application without doing huge refactoring.

In this post I just want to share the short steps I did during researching implementing session state on a database.
First of all open command prompt and navigate to .NET framework installation path. In this case, my installation path is <code>C:\Windows\Microsoft.NET\Framework64\v4.0.30319></code>

We will use a tool called <code>aspnet_regsql.exe</code> that shipped with .NET framework installation to generate database that will be used to store session state. To display detail argument needed by this tool we can use the following command:

``` vctreestatus aspnet_regsql
C:\Windows\Microsoft.NET\Framework64\v4.0.30319>aspnet_regsql.exe /?
```

``` vctreestatus generate session state database
C:\Windows\Microsoft.NET\Framework64\v4.0.30319>aspnet_regsql.exe -ssadd -d {databaseName} -S {server\instance} -sstype c -U {username} -P {password}
```

The command will create database, tables and store procedures.
In web.config, don’t forget to add session state configuration by pointing the generated database.

``` xml session state configuration
<system.web>
    <sessionstate mode="SQLServer" allowcustomsqldatabase="true" sqlconnectionstring="Data Source={server\instance};Initial Catalog={databaseName}; user={username}; password={password}" cookieless="false" timeout="20">

  </sessionstate>
</system.web>
```
To test our session storage, just create simple aspx page.

``` aspx-cs sample.aspx
<asp:button id="btnAddSession" runat="server" text="Add Session" onclick="btnAddSession_Click"/>

<asp:button id="btnRemoveSession" runat="server" text="Remove Session" onclick="btnRemoveSession_Click"/>

<br>
<asp:label id="ctlLblSessionDisplay" runat="server" text=""></asp:label>

```

``` c# sample.aspx.cs
protected void Page_Load(object sender, EventArgs e)
{
    if (Session[Constant.SessionKey] != null)
        ctlLblSessionDisplay.Text = Session[Constant.SessionKey].ToString();
    else
        ctlLblSessionDisplay.Text = "NO Session";
}

protected void btnAddSession_Click(object sender, EventArgs e)
{
    Session.Add(Constant.SessionKey, "Hello");
}

protected void btnRemoveSession_Click(object sender, EventArgs e)
{
    Session.Remove(Constant.SessionKey);
}
```
