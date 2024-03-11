---
title: "OpenSpartan Workshop 1.0.3 (Escharum) Is Now Available"
slug: openspartan-workshop-new-release-escharum
description: "Another week, another release of OpenSpartan Workshop (1.0.3, build ESCHARUM-03052024)! This one was a fun to build because it introduces a few improvements that will delight its users."
summary: "The latest release of OpenSpartan Workshop is here!"
date: 2024-03-08
lastmod: 2024-03-08
draft: false
weight: 50
categories: []
tags: []
contributors: []
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Another week, another release of OpenSpartan Workshop (1.0.3, build `ESCHARUM-03052024`)! This one was a fun to build because it introduces a few improvements that will delight its users.

[**🚀 Download latest version**](https://github.com/OpenSpartan/openspartan-workshop/releases/download/1.0.3/OpenSpartan.Workshop.Installer.Bundle.exe)

## Ranked data tracking

First and foremost, with the latest release of OpenSpartan Workshop you will have an easier way to keep track of your ranked playlist progression. Your ranked progression, current CSR, and gained or lost CSR is now displayed in match overview. Not only that, but you will also be able to _visually_ inspect the CSR change post-match by clicking on a match row and seeing the progression, similar to how you can track it in-game.

![Animated GIF showing ranked match data in match overviews](images/blog/openspartan-workshop-new-release-escharum/ranked-demo.gif)

## Medal data

If, like me, you want to [**query medal data locally**](/docs/workshop/guides/how-to-query-local-data/), you might've realized that you need medal name IDs to be able to correctly identify the right medals. Worry not - you can now click on a medal and see that value right away.

{{< figure class="rounded-3 h-auto" src="images/blog/openspartan-workshop-new-release-escharum/medal-name-id.png" alt="Screenshot showing medal metadata in OpenSpartan Workshop." caption="Screenshot showing medal metadata in OpenSpartan Workshop." >}}

I should also mention that the flyouts will now time out after eight seconds as to not clutter the screen.

Another added improvement is that medal details are now _also_ shown in match details, so you don't have to go far to see where you earned that **Grand Slam** purple badge.

![Animated GIF showing medal data in match overview](images/blog/openspartan-workshop-new-release-escharum/medal-match-details.gif)

## Small improvements and bug fixes

In the mix, there were also a few bug fixes and performance improvements that are listed in the [**release notes**](https://github.com/OpenSpartan/openspartan-workshop/releases/tag/1.0.3). The app is becoming more stable and fast, which is nice for those of you with _a lot_ of matches in your history.

## Exploration notebook

Oh yeah, and as an aside - I built a [**Jupyter Notebook**](https://github.com/OpenSpartan/notebooks) for you Python nerds who want to see how to explore the SQLite data from your local box. It has example SQL queries along with [**Seaborn**](https://seaborn.pydata.org/)-based visualizations. Try it out once you have the data collected locally. All you need to do is substitute my XUID with yours. The sky is the limit with what kinds of insights you can extract here!

![Animated GIF showing medal data in match overview](images/blog/openspartan-workshop-new-release-escharum/openspartan-notebook.gif)

## Feedback and improvements

That's about it for this release! [**Go to the docs**](/docs/workshop/guides/get-started/) to get started and download the latest version. If you have any feedback, such as bug reports or feature requests, [**open an issue**](https://github.com/OpenSpartan/openspartan-workshop/issues)!
