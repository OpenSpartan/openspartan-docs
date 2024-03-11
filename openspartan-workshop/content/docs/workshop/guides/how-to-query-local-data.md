---
title: "How To Query Local Data"
slug: how-to-query-local-data
description: "How to run SQL queries against the data in the local OpenSpartan Workshop SQLite database."
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

You can run queries against the [local database](../understanding-the-local-database) just like you would against any SQLite database. My preferred approaches are using [DB Browser for SQLite](https://sqlitebrowser.org/) or a [Jupyter notebook](https://jupyter.org/) (mainly because that way it's easier to visualize results in graphs).

A few of the queries below are used internally by OpenSpartan Workshop to generate in-app visualizations (like the match data). But why limit yourself to the application when you can slice and dice the data in whichever way you want, giving you even deeper insights.

## Jupyter notebook

If you are a Python user, you might want to start with the [Jupyter Notebook collection](https://github.com/OpenSpartan/notebooks) that I maintain. It showcases both SQL query examples, as well as visualizations using [Seaborn](https://seaborn.pydata.org/).

## Example queries

To get you started, here are a few sample queries that you can use to explore the content in the database. Substitute my XUID below (when looking for `PlayerId`) for your own. Your XUID is in the format of `xuid(YOUR_PLAYER_NUMERIC_ID)` and can be obtained by:

1. Looking at the name of the database file.
2. Looking into OpenSpartan Workshop settings - XUID is displayed after you log in.
3. Looking inside the `ServiceRecordSnapshots` table

{{< callout context="caution" title="Caution" icon="alert-octagon" >}}
Running queries yourself while OpenSpartan Workshop is trying to get the data _or_ insert data in the database may result in OpenSpartan Workshop instability and/or data corruption. Don't do both at the same time.
{{< /callout >}}

{{< callout context="danger" title="Danger" icon="alert-octagon" >}}
All the queries below are non-destructive - that is, they don't alter any of the data, but because the database is in your possession you can _also_ run destructive queries (e.g., modify data or remove entries). While I can't stop you from doing that, know that it also **may result in OpenSpartan Workshop instability**, so do that at your own risk.
{{< /callout >}}

### Get all played matches and team MMR

After every match, I like looking at the Matchmaking Rank (MMR) for my team to get an idea as to how we stacked up against our opponents.

```sql
WITH RAW_MATCHES AS (
    SELECT
        MatchId,
        Teams,
        json_extract(MatchInfo, '$.StartTime') StartTime,
        json_extract(MatchInfo, '$.Duration') Duration,
        json_extract(MatchInfo, '$.GameVariantCategory') GameVariantCategory,
        json_extract(MatchInfo, '$.MapVariant.AssetId') Map,
        json_extract(MatchInfo, '$.Playlist.AssetId') Playlist,
        json_extract(MatchInfo, '$.UgcGameVariant.AssetId') UgcGameVariant,
        json_extract(value, '$.Rank') AS Rank,
        json_extract(value, '$.Outcome') AS Outcome,
        json_extract(value, '$.LastTeamId') AS LastTeamId,
        json_extract(value, '$.ParticipationInfo') AS ParticipationInfo,
        json_extract(value, '$.PlayerTeamStats') AS PlayerTeamStats
    FROM
        MatchStats
        INNER JOIN json_each(Players) ON json_extract(json_each.value, '$.PlayerId') = "xuid(YOUR_XUID)"
),
MATCH_DETAILS AS (
    SELECT
        MatchId,
        json_extract(value, '$.Result.TeamMmr') AS TeamMmr
    FROM
        PlayerMatchStats
        INNER JOIN json_each(PlayerStats) ON json_extract(json_each.value, '$.Id') = "xuid(YOUR_XUID)"
)
SELECT
    RM.MatchId,
    RM.Teams,
    RM.StartTime,
    RM.Duration,
    RM.Rank,
    RM.Outcome,
    RM.LastTeamId,
    RM.ParticipationInfo,
    RM.PlayerTeamStats,
    RM.GameVariantCategory,
    M.PublicName AS Map,
    P.PublicName AS Playlist,
    GV.PublicName AS GameVariant,
    MD.TeamMmr AS TeamMmr
FROM
    RAW_MATCHES RM
    LEFT JOIN Maps M ON M.AssetId = RM.Map
    LEFT JOIN Playlists P ON P.AssetId = RM.Playlist
    LEFT JOIN GameVariants GV ON GV.AssetId = RM.UgcGameVariant
    LEFT JOIN MATCH_DETAILS MD ON MD.MatchId = RM.MatchId
GROUP BY
    RM.MatchId
ORDER BY
    RM.StartTime DESC;
```

This query can take a bit of time, depending on your machine configuration, but you will end up with all the match metadata _along_ with the MMR:

{{< img class="rounded-3 h-auto" src="images/docs/long-query-matches.png" alt="Career overview screenshot in OpenSpartan Workshop." >}}

### Breakdown of wins and losses month-over-month

Gives you a perspective of how you're improving month-over-month.

```sql
WITH RAW_COLLECTION AS (
    SELECT
        strftime('%Y-%m', date(json_extract(MatchInfo, '$.StartTime'))) AS StartTime,
        MatchId,
        json_extract(value, '$.Outcome') AS Outcome
    FROM
        MatchStats
        INNER JOIN json_each(Players) ON json_extract(json_each.value, '$.PlayerId') = 'xuid(YOUR_XUID)'
)

SELECT
    StartTime,
    COUNT(CASE WHEN Outcome = 2 THEN 1 END) AS Wins,
    COUNT(CASE WHEN Outcome = 3 THEN 1 END) AS Losses,
    COUNT(CASE WHEN Outcome = 1 THEN 1 END) AS Ties,
    COUNT(CASE WHEN Outcome = 4 THEN 1 END) AS NoFinishes
FROM
    RAW_COLLECTION
GROUP BY
    StartTime
ORDER BY
    StartTime ASC;
```

### Breakdown of all KDA values across all matches

This is particularly helpful if you want to compute your median KDA.

```sql
WITH RAW_COLLECTION AS (
    SELECT
        strftime('%Y-%m', date(json_extract(MatchInfo, '$.StartTime'))) AS StartTime,
        MatchId,
        json_extract(value, '$.PlayerTeamStats') AS PlayerSnapshot
    FROM
        MatchStats
        INNER JOIN json_each(Players) ON json_extract(json_each.value, '$.PlayerId') = 'xuid(YOUR_XUID)'
)

SELECT
    json_extract(value, '$.Stats.CoreStats.KDA') AS KDA
FROM
    RAW_COLLECTION
    CROSS JOIN json_each(PlayerSnapshot);
```

### Matches and outcomes with another player

This was a request [from Reddit](https://www.reddit.com/r/CompetitiveHalo/comments/1b3be4g/comment/kswun4h/?utm_source=reddit&utm_medium=web2x&context=3) Allows you to find out the outcomes of matches where you were on the same team with another player.

{{< callout context="note" title="Note" icon="info-circle" >}}
You will need to know the Xbox User ID (XUID) of the other player. It's _not_ the Gamertag, but can be converted from the Gamertag.
{{< /callout >}}

```sql
WITH PlayerInfo AS (
SELECT
        t.MatchId,
        CAST(JSON_EXTRACT(tp.value, '$.TeamId') AS INTEGER) AS TeamId,
        CAST(JSON_EXTRACT(tp.value, '$.Outcome') AS INTEGER) AS TeamOutcome,
        JSON_EXTRACT(p.value, '$.PlayerId') AS PlayerId,
        CAST(JSON_EXTRACT(p.value, '$.LastTeamId') AS INTEGER) AS LastTeamId
    FROM MatchStats t
    LEFT JOIN JSON_EACH(t.Teams) AS tp ON 1=1
    LEFT JOIN JSON_EACH(t.Players) AS p ON 1=1
    WHERE TeamId = LastTeamId
)

SELECT
    pi1.MatchId,
    pi1.TeamId,
    pi1.TeamOutcome AS Outcome
FROM PlayerInfo pi1
JOIN PlayerInfo pi2 ON pi1.MatchId = pi2.MatchId AND pi1.TeamId = pi2.TeamId
WHERE pi1.PlayerId = 'xuid(YOUR_XUID)' AND pi2.PlayerId = 'xuid(FRIEND_XUID)' AND pi1.PlayerId != pi2.PlayerId;
```

The returned data will include a numeric representation of the outcome. Refer to [Dissecting Match Metadata](/docs/workshop/advanced/dissecting-match-metadata/) to learn about possible outcomes.
