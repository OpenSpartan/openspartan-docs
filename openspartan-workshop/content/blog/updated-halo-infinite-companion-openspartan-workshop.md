---
title: "OpenSpartan Workshop 1.0.6 (Crucible) Ready for Download"
slug: updated-halo-infinite-companion-openspartan-workshop
description: "A new version of OpenSpartan Workshop is released, adding support for The Exchange, ranked tier counterfactuals, comprehensive event tracking, a faster match search, and more."
summary: "A new version of OpenSpartan Workshop is released, adding support for The Exchange, ranked tier counterfactuals, comprehensive event tracking, a faster match search, and more."
date: 2024-06-08
lastmod: 2024-06-08
draft: false
weight: 50
categories: [OpenSpartan Workshop]
tags: [halo-infinite,openspartan-workshop,open-source]
contributors: [Den Delimarsky]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

You can catch up with the [latest release notes on GitHub](https://github.com/OpenSpartan/openspartan-workshop/releases/tag/1.0.6). You can also [download the latest release right away](https://github.com/OpenSpartan/openspartan-workshop/releases/download/1.0.6/OpenSpartan.Workshop.Installer.Bundle.exe).

You can also watch the video overview of the tool!

{{< youtube JcbDr6cATOc >}}

If you thought that OpenSpartan Workshop hasn't had updates in a few months, you thought wrong! This one is _packed_ with a few new features that I am really excited about. As with all other releases, this software is, in my eyes, always somewhat experimental - things _might_ be a bit wonky as I stabilize the application (a bit on that later), but it's already working pretty well for some core scenarios.

But, first things first - why the jump from `1.0.3` to `1.0.6`? Thanks to some post-release testing, there were a few problems that bubbled up in `1.0.4` that had to be hotfixed over several releases. This also helped me fine-tune the release pipeline to catch some of the regressions early. Hence, the currently supported release is `1.0.6`.

So, long story longer - what's new?

## Battle pass/operation/event metadata

When I first [implemented the support for battle pass data](https://www.openspartan.com/blog/announcing-first-release-openspartan-workshop/) I missed one critical step, and that is the ability to see details about a specific item. Now, when you click on an item, you will get its name and description, and for currency you will see the currency amount.

![Battle Pass metadata shown in OpenSpartan Workshop](images/blog/updated-halo-infinite-companion-openspartan-workshop/battlepass-metadata.gif)

There is some data still missing, like listing the _item type_ that I want to [add in a future update](https://github.com/OpenSpartan/openspartan-workshop/issues/40).

## Tier counterfactuals for ranked matches

One piece of data that I found lacking in existing experiences that _does exist_ in the Halo Infinite API is _tier counterfactuals_ - the expectations of performance for a given match for a specific tier. Say, you complete a Ranked Arena match and go "Wow, I performed like I am an Onyx, but I am still in Platinum." Well, if you look inside the Tier Counterfactuals section in the match overview, you can now see what was expected of someone in the Onyx rank (among other ranks) in a given match.

{{< figure class="rounded-3 h-auto" src="images/blog/updated-halo-infinite-companion-openspartan-workshop/tier-counterfactuals.png" alt="Tier counterfactuals for a ranked match in OpenSpartan Workshop." caption="Tier counterfactuals for a ranked match in OpenSpartan Workshop." >}}

## Ranked data aggregated in its own view

If you are a fan of ranked game modes, this is where you can now go to see your standing across all ranked playlists. The data includes your rank, progression in that rank, maximum rank achieved this season, and maximum rank achieved in the player's lifetime.

{{< figure class="rounded-3 h-auto" src="images/blog/updated-halo-infinite-companion-openspartan-workshop/ranked-view.png" alt="Ranked performance data in OpenSpartan Workshop." caption="Ranked performance data in OpenSpartan Workshop." >}}

I am constantly in Platinum, but ventured into Diamond with the Tactical Slayer playlist. Go figure, I guess my one-click reactions are better than my ability to strafe against a Bandit.

## Loose search for faster match updates

In previous releases, every time you restarted the app or asked for a match data refresh, OpenSpartan Workshop would try to check against the Halo Infinite API every single match that you played and whether it already exists in the [local SQLite database](https://www.openspartan.com/docs/workshop/guides/how-to-query-local-data/). There is just one slight problem with that - it can take a _really long time_ if you have thousands of matches played (I would know). To mitigate this problem, I now introduced a `Loose Match Search` mode. It's disabled by default, but can be flipped on in application settings.

{{< figure class="rounded-3 h-auto" src="images/blog/updated-halo-infinite-companion-openspartan-workshop/loose-match-search.jpg" alt="Loose match search in OpenSpartan Workshop." caption="Loose match search in OpenSpartan Workshop." >}}

I am keeping it off by default for now because it's experimental - I want to see where it fails and whether it's robust enough to pick up many recent matches at once. One thing you should know about the _current_ (full) match search is that it aggregates _all_ match, map, and playlist metadata on every pass. This is done through a combination of using the Halo Infinite API match search endpoint and then querying for additional information on every entity. The search starts with the most recent matches and moves down the history of matches you played. So, if, for example, you decide to start scanning for matches on the first run and then abandon half-way, and then enable `Loose Match Search`, those matches at the tail end that never were processed won't be picked up.

That's mainly because `Loose Match Search` has a threshold of successfully stored matches - it pulls a batch of 25 matches at a time and tries to check if those matches are already in the database. If it hits 100 continuous matches that are already in the database, it assumes that there is no need to search for more because we only want to pick up the _latest matches that are not in the database_.

You can see why I want to test this thoroughly before it becomes the default

## Sane settings UI

On the topic of settings, the actual user interface (UI) for settings now matches that of the Windows Settings app and not some half-baked monstrosity that I put together as a hacky band-aid.

{{< figure class="rounded-3 h-auto" src="images/blog/updated-halo-infinite-companion-openspartan-workshop/settings-ui.png" alt="New settings UI in OpenSpartan Workshop." caption="New settings UI in OpenSpartan Workshop." >}}

## Support for authentication without using the Windows broker

If you used OpenSpartan Workshop before, you knew that it defaults to using the [Windows authentication broker](https://learn.microsoft.com/entra/msal/dotnet/acquiring-tokens/desktop-mobile/integrated-windows-authentication), which in turn connected your personal Microsoft account to Windows (if you haven't already). There were a few customers that didn't want to do that, so to enable this scenario you can now _disable authentication with the broker_ and instead use the web browser.

This setting is not enabled by default, but you can control it from the settings view (you will need to log out first) or by modifying the `settings.json` file (you can find it in `%LOCALAPPDATA%\OpenSpartan.Workshop`) - set `usebroker` to `false`.

## Show items from The Exchange

You can go to OpenSpartan Workshop to see both the expiration date of the current Exchange offering, as well as all of the items currently available for purchase (along with their prices in Spartan Points).

![The Exchange metadata shown in OpenSpartan Workshop](images/blog/updated-halo-infinite-companion-openspartan-workshop/the-exchange.gif)

In the future, I am looking at adding the ability to [purchase items through the app](https://github.com/OpenSpartan/openspartan-workshop/issues/34).

## List events in addition to operations

Remember how before the current battle pass model (that involves operations) we had _events_ like [Cyber Showdown](https://www.halowaypoint.com/news/cyber-showdown-event-launch), [Noble Intention](https://www.halowaypoint.com/news/noble-intention-event-launch) (although in the API metadata it's listed as _Noble Intentions_), [Interference](https://www.halowaypoint.com/news/season-2-lone-wolves-launch), and others? Did you ever want to go back and see what items you missed out from those?

You now can:

![Event metadata shown in OpenSpartan Workshop](images/blog/updated-halo-infinite-companion-openspartan-workshop/events.gif)

## Season calendar

Last, but most definitely not least, another experimental feature that I added (and will continue working on) is the season calendar.

![Season calendar shown in OpenSpartan Workshop](images/blog/updated-halo-infinite-companion-openspartan-workshop/calendar.gif)

It allows you to conveniently track operations, events, and most importantly - _ranked seasons_ (also known as CSR seasons). The latter can help you understand when the next rank reset will happen (and when previous ones happened, too). The UI is _a bit_ rough, but I am working on [cleaning it up more in future releases](https://github.com/OpenSpartan/openspartan-workshop/issues/39).

## Quality-of-life updates

- **Missing medals rendered**. If you ever got a `Perfection` medal in a custom game, that medal won't show up in the broader medal collection. That also meant that it won't show up when you looked at match details in the match table. Not anymore. I now download all medal images properly ahead of time, so there won't be any missing match achievements.
- **No arrows rendered in the match view if no expected deaths or kills are registered**. On the topic of custom games, they also do not track expected kills or deaths, unlike other matchmade games. That means that there is no point in rendering the up/down arrows since there are no expectations to match. The match table is now cleaner if you play a lot of custom games or minigames.
- **Medals actually refreshed on application launch**. The initial battery of medals is loaded from the service record API call. However, because the service record API call was executed in a batch with other calls, it might be possible that medals would take a bit extra time to show up properly. Now, I ensure that the service record is loaded and bound correctly right away.
- **Medal label not rendered when there are no medals**. If you had a somewhat disappointing match and earned no medals, there is no reason to remind you about that with a stray label that says `Medals` in the match overview. It's now gone.
- **Better logging for easy diagnostics**. This doesn't necessarily affect the _consumer_ quality of life, but it does make it easier for me to diagnose issues because I can now see specific functions called out in the logs that allow me to know exactly where to look. And of course, logging is disabled by default. You only need it enabled if you are trying to track down specific issues.
- **Match acquisition logic now has a retry queue**. Who knew - trying to get _thousands_ of matches for a player at once might result in _some_ missed entries due to a myriad of possible reasons, like a temporary internet hiccup. There is now a retry queue where I track failed calls to get match data and retry getting said data again a few times before fully giving up.
- **Dialog shown when logging out matches modern Windows UI**. That old-school Windows 95-like message box is now replaced with a more modern, Windows 11-like prompt to log out.
- **Minigame modes (like [Survive The Undead](https://www.halowaypoint.com/news/combat-workshop-survive-the-undead)) are now correctly rendered in match table**. If you were using one of the previous versions of OpenSpartan Workshop, it depended on a version of a Halo Infinite API wrapper library that I built that _did not_ have the right records for minigames, and you ended up with a number (like `42`) rendered in the match table. That is no longer the case - you can now see the proper game type.
- **Currency items now display their title in the battle pass view**. I don't expect you to know the difference between XP Grant and XP Boost, especially when their icons are _so similar_. That means that titles were needed. And so - titles were added for consumable items in the battle pass view.
- **Nameplates now render again**. In previous releases, there was a regression where nameplates would not render, regardless of [whether they existed in the API or not](https://den.dev/blog/openspartan-battlepass/#a-horse-with-no-nameplate).
- **Matches ordered by their end time rather than start time**. This was a little bit embarrassing - matches were ordered by when they started, but what I really needed to see is an order based on when the match ended. This has been rectified.
- **Bronze ranks are now properly displayed**. No disrespect to anyone in the Bronze tier. I just forgot to include the necessary images in the configuration list of images to download. You will now see those properly in the match table, match description, and ranked progression view.

## Feedback and improvements

That's it for this release! [**Go to the docs**](/docs/workshop/guides/get-started/) to get started and download the latest version. If you have any feedback, such as bug reports or feature requests, [**open an issue**](https://github.com/OpenSpartan/openspartan-workshop/issues)!
