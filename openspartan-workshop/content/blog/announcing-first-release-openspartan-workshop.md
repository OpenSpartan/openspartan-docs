---
title: "Announcing OpenSpartan Workshop 1.0"
slug: announcing-first-release-openspartan-workshop
description: "The first version of OpenSpartan Workshop is officially out."
summary: "The first version of OpenSpartan Workshop is officially out."
date: 2024-02-21
lastmod: 2024-02-21
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

Hello there! After a long wait, the first release of **OpenSpartan Workshop** is finally out. You can browse the code and download the first release [on GitHub](https://github.com/OpenSpartan/openspartan-workshop).

[**🚀 Download latest version**](https://github.com/OpenSpartan/openspartan-workshop/releases/download/1.0.0/OpenSpartan.Workshop.Installer.Bundle.exe)

![Animated GIF of OpenSpartan Workshop 1.0](images/landing-header.gif)

## Overview

The current implementation is Windows-only, but I am thinking of best ways of bringing it to other platforms like macOS, iOS, and Android. Maybe even Linux in the not-so-distant feature. This all will come after I _significantly_ stabilize the Windows experience.

This release enables a few things:

1. 😲 **Career stats**. At a glance, see what your overall performance is and how far you are from the [Hero rank](https://www.halowaypoint.com/news/career-rank-overview-season-4).
2. 📊 **Match stats**. You can easily track every single match, along with your performance and, most importantly, the team Matchmaking Rank (MMR).
3. 🎖️ **Medal stats**. Who doesn't like to see their top medals. It's all one-click away with OpenSpartan Workshop.
4. ⚔️ **Battle pass progress**. At a glance, you can view all of the currently active operations and how you progress along the ladder.

Above all, and this is what I am most excited with this tool - all of the data is collected through the Halo Infinite API and stored locally in a SQLite database. This means that you can very quickly analyze it and build any visualizations or reports, just like I did [on my blog](https://den.dev/halo).

Consider this release a _very early beta_ - there are a lot of unpolished pieces and quite a few stability quirks that I will iron out in the coming months. In the meantime, it is still a great avenue to both collect and track your in-game performance.

## Get started

[**Go to the docs**](/docs/workshop/guides/get-started/)! No, seriously - I am actively working on documenting the end-to-end experience, and will be putting more content in that section of the site, so you can always build a more in-depth understanding of the data and how to use it.

There are more features that will be lighting up soon that I am excited to showcase, but in the meantime - head over to the [GitHub repository](https://github.com/OpenSpartan/openspartan-workshop) and download the [latest release](https://github.com/OpenSpartan/openspartan-workshop/releases/).

If you have any feedback, such as bug reports or feature requests, [open an issue](https://github.com/OpenSpartan/openspartan-workshop/issues)!
