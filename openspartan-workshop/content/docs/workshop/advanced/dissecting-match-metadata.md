---
title: "Dissecting Match Metadata"
slug: dissecting-match-metadata
description: "This is a reference on match metadata captured in the local SQLite database."
summary: ""
date: 2024-03-01
lastmod: 2024-03-01
draft: false
menu:
  docs:
    parent: ""
    identifier: "example-ee51430687e728ba6e68dea3359133ad"
weight: 910
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

This is a reference on match metadata captured in the local SQLite database.

## MatchStats Table

### Players Column

Structured as JSON. Array of players participating in a match. Every player has a number of attributes associated with them:

| Property | Description |
|:---------|:------------|
| `BotAttributes`     | Only applicable to players that were bots. |
| `LastTeamId`        | Numeric identifier of the team the player was last on. |
| `Outcome`           | Numeric identifier of the outcome of the match for a given player and their team.<br/>`1` - Tie<br/>`2` - Win<br/>`3` - Loss<br/>`4` - Did not finish |
| `ParticipationInfo` | General metadata about participation in the match. Includes:<br/>- Time the player joined the match (`FirstJoinedTime`)<br/>- Whether the player joined a match that was already played (`JoinedInProgress`)<br/>- Time at which the player left the match if the player quit (`LastLeaveTime`)<br/>- Whether the player left the match in progress (`LeftInProgress`)<br/>- Whether the player was present at the start of the match (`PresentAtBeginning`)<br/>- Whether the player was there when match completed (`PresentAtCompletion`)<br/>- Time played.|
