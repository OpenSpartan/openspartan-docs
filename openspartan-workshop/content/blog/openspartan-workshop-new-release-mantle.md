---
title: "OpenSpartan Workshop 1.0.1 (Mantle) Is Now Available"
slug: openspartan-workshop-new-release-mantle
description: "Right after the release of the first version of OpenSpartan Workshop, I am following-up with an update that adds some quality-of-life improvements and some minor new functionality. You can get the latest release on GitHub."
summary: "Right after the release of the first version of OpenSpartan Workshop, I am following-up with an update that adds some quality-of-life improvements and some minor new functionality. You can get the latest release on GitHub."
date: 2024-02-29
lastmod: 2024-02-29
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

Right after the [**release**](/blog/announcing-first-release-openspartan-workshop) of the first version of [**OpenSpartan Workshop**](/docs/workshop/guides/get-started/), I am following-up with an update that adds some quality-of-life improvements and some minor new functionality. You can get the latest release [**on GitHub**](https://github.com/OpenSpartan/openspartan-workshop/releases/tag/1.0.1).

## Bug fixes

There were a number of small (but somewhat annoying) papercuts that are now addressed:

- Performance of navigation from view to view is now better. This is mainly done through built-in WinUI caching mechanisms for views. I _think_ I can push this even further, but that's going to be saved for a future release.
- By proxy of the fix above, and because views are properly cached now, navigating to, say, the **Battle Pass** view results in the view "remembering" the tab you're on. This means that going back is going to take you exactly where you were before.
- One of the logging pieces was not respecting the logging setting (enabled or disabled). It's now properly set up so you won't end up with unnecessary logging files.

## Expected deaths and kills in match overviews

The **Matches** overview now not only shows the match performance but also how [TrueSkill 2](https://www.microsoft.com/en-us/research/uploads/prod/2018/03/trueskill2.pdf) _thought_ you would perform. This is all pulled from the Halo Infinite API.

{{< figure class="rounded-3 h-auto" src="images/blog/openspartan-workshop-new-release-mantle/expected-values.png" alt="Expected kill and death values rendered in the match overview." caption="Expected kill and death values rendered in the match overview." >}}

As a bonus point, you also get a very clear visual indicator of whether you underperformed, overperformed, or met expectations.

## Medal descriptions

Previously, you could see the medals you earned and their names, but there were _zero_ clues to tell you what the medal actually was for. Now, when you click on a medal, you will see its description.

{{< figure class="rounded-3 h-auto" src="images/blog/openspartan-workshop-new-release-mantle/medal-description.png" alt="Medal descriptions seen in the Medals view." caption="Medal descriptions seen in the Medals view." >}}

## Browsing matches based on medals

Piggybacking on the medal description fly-out, you can now also see the matches where you earned that particular medal. This is not something that today is possible through the Halo Infinite API. OpenSpartan Workshop makes it possible because we have a local database that we can query in whichever way we want.

![Animated GIF showing how to navigate to matches based on a medal](images/blog/openspartan-workshop-new-release-mantle/medal-matches.gif)

Because I want to make it easier for folks to also query their own data from the SQLite database, when you navigate to a medal-based match view, you also get the medal `NameId` value in the header.

{{< figure class="rounded-3 h-auto" src="images/blog/openspartan-workshop-new-release-mantle/medal-nameid.png" alt="Medal NameId value rendered next to the medal name." caption="Medal NameId value rendered next to the medal name." >}}

## Feedback and improvements

That's about it for this release! And _because_ I released this on Leap Day, you can be assured that this app will work even on February 29th. [**Go to the docs**](/docs/workshop/guides/get-started/) to get started and download the latest version. If you have any feedback, such as bug reports or feature requests, [**open an issue**](https://github.com/OpenSpartan/openspartan-workshop/issues)!

