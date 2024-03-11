---
title: "Understanding The Local SQLite Database"
slug: understanding-the-local-database
description: "An overview of the structure of the OpenSpartan Workshop local SQLite database."
summary: ""
date: 2024-02-21
lastmod: 2024-02-21
draft: false
menu:
  docs:
    parent: ""
    identifier: "example-6a1a6be4373e933280d78ea53de6158e"
weight: 810
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

When you _first_ log in to OpenSpartan Workshop, a new database is created that is named after the **player Xbox User ID (XUID)**.

You can get to the database by running the following in the **Run...** dialog in Windows (<kbd>Win</kbd> + <kbd>R</kbd>) or in the Terminal:

```powershell
%LOCALAPPDATA%\OpenSpartan.Workshop\data
```

{{< img class="rounded-3 h-auto" src="images/docs/database-file.png" alt="The database file highlighted in Windows Explorer." >}}

The database itself doesn't have anything Windows-specific in it and can be easily used on any platform where SQLite is supported. For example, I could use it with [DB Browser for SQLite](https://sqlitebrowser.org/) on macOS:

{{< img class="rounded-3 h-auto" src="images/docs/db-browser-macos.png" alt="DB Browser for SQLite running on macOS." >}}

The following tables are currently available:

| Table Name | Description |
|:-----------|:------------|
| `EngineGameVariants` | Metadata about engine game variants for all games in which the logged in player ever participated. |
| `GameVariants` | Metadata about game variants for all games in which the logged in player ever participated. |
| `InventoryItems` | Metadata about inventory items that are available in Halo Infinite. |
| `Maps` | Metadata about maps for all games in which the logged in player ever participated. |
| `MatchStats` | High-level match statistics for every match the logged in player ever played. |
| `OperationRewardTracks` | Metadata about available operations. |
| `OwnedInventoryItems` | Metadata about inventory items owned by the logged in player. |
| `PlayerMatchStats` | Detailed match statistics for every match the logged in player ever played. Can be joined with `MatchStats` for a full picture of match performance. |
| `PlaylistMapModePairs` | Metadata about map/mode pairs for all games in which the logged in player ever participated. |
| `Playlists` | Metadata about playlists for all games in which the logged in player ever participated. |
| `ServiceRecordSnapshots` | Snapshots of the player service record, captured every time OpenSpartan Workshop starts. |

Each table has a `ResponseBody` column that contains the full API response at the time of data acquisition, with subsequent columns derived from the JSON response. When [querying](/docs/workshop/guides/how-to-query-local-data), you can either use the full JSON response or specific columns. Performance-wise, using extracted columns is going to be better - the full response is there mostly for reference.
