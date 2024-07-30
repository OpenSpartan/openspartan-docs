---
title: "Getting Started with OpenSpartan Workshop"
slug: get-started
description: "Getting started with OpenSpartan Workshop to analyze your Halo stats."
summary: "Getting started with OpenSpartan Workshop to analyze your Halo stats."
date: 2024-03-01
lastmod: 2024-03-01
draft: false
menu:
  docs:
    parent: ""
    identifier: "example-6a1a6be4373e933280d78ea53de6158e"
weight: 100
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

To get started with OpenSpartan Workshop, you will first need to make sure that you install the latest version of the application. You can get it [from GitHub](https://github.com/OpenSpartan/openspartan-workshop/releases). Look for `OpenSpartan.Workshop.Installer.Bundle.exe`.

[**🚀 Download latest version**](https://github.com/OpenSpartan/openspartan-workshop/releases/download/1.0.8/OpenSpartan.Workshop.Installer.Bundle.exe)

You can watch the overview video about OpenSpartan Workshop here:

{{< youtube JcbDr6cATOc >}}

{{< callout context="note" title="Note" icon="info-circle" >}}
The installer will also attempt to install [.NET Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/8.0), and the latest [Windows App SDK](https://learn.microsoft.com/windows/apps/windows-app-sdk/downloads).
{{< /callout >}}

Once the application is installed you will be prompted to sign in with your **Microsoft account**. This is the same account that you use to sign in on your Xbox or PC where you're playing Halo.

{{< img class="rounded-3 h-auto" src="images/docs/new-auth-launch.png" alt="Authentication flow in OpenSpartan Workshop." >}}

{{< callout context="note" title="Note" icon="info-circle" >}}
OpenSpartan Workshop currently only supports Halo Infinite, with future plans to expand to Master Chief Collection. There are no concrete dates on this yet.
{{< /callout >}}

Once you're signed in, you should be able to see your career page, displaying your overall performance statistics.

{{< img class="rounded-3 h-auto" src="images/docs/career-overview.png" alt="Career overview screenshot in OpenSpartan Workshop." >}}

When you start OpenSpartan Workshop, the application will query all matches that you played and store them in the local SQLite database, located in the application data folder.

You can find the database by going to `%LOCALAPPDATA%\OpenSpartan.Workshop\data`, where there will be a `.db` file associated with your Xbox User ID (XUID).

{{< callout context="note" title="Note" icon="info-circle" >}}
The XUID is different from your gamertag. Unlike the gamertag, which can be changed, the XUID is immutable.
{{< /callout >}}

The `.db` file can be opened and queried through any compatible SQLite tool, such as [DB Browser for SQLite](https://sqlitebrowser.org/).

## Asynchronous data loading

When you first start OpenSpartan Workshop, there are a few things happening in the background, such as loading your career data, populating the medal data, and scanning the changes in available battle passes. The data is obtained through the Halo Infinite API and is stored in designated tables within the local database.

### Career

Your career should load fairly fast - within the first few seconds after startup, and is rendered on the first page.

### Matches

The match list is populated after startup automatically and is displayed in the **Matches** view.

{{< img class="rounded-3 h-auto" src="images/docs/matches.png" alt="Match overview in OpenSpartan Workshop." >}}

When the application is started to get the most accurate data about the player matches two things will happen:

1. **Query the API for all matches the player has ever played**. This is done in 25 match increments (that is the API limitation).
2. **Get metadata based on the match delta**. OpenSpartan Workshop will compare the list it obtained by querying the player match history with the list of already stored matches in the local database and get the match statistics and map/mode metadata stored locally for the ones that are not there already.

Depending on how often you're using OpenSpartan Workshop, this process may take some time. Because for every match that is not in the database the API is being queried, this is a slower process than just displaying the most recent matches.

While this is a drawback of the current implementation, the benefit here is that once all matches are loaded they can be inspected in the database without ever touching the API again.

### Medals

Medal metadata is also populated in the local database, providing accurate counts for all the medals you've ever earned through your career.

{{< img class="rounded-3 h-auto" src="images/docs/medals.png" alt="Medal overview in OpenSpartan Workshop." >}}

### Battle Pass/Operation Progression

Lastly, you also get to see how far along you are in the currently available operations. These are also loaded sequentially, so you might have a short delay between starting the application and seeing how far along you are in every single operation that is available in the current game release.

{{< img class="rounded-3 h-auto" src="images/docs/battlepass.png" alt="Battle pass overview in OpenSpartan Workshop." >}}


## Further Reading

- Familiarize yourself with the [database structure](/docs/workshop/guides/understanding-the-local-database).
- Have a peek at [how to query local data](/docs/workshop/guides/how-to-query-local-data).
