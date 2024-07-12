---
title: "OpenSpartan Workshop 1.0.7 (Cylix) Is Now Available"
slug: openspartan-workshop-latest-release-cylix
description: ""
summary: "The latest release of OpenSpartan Workshop is here!"
date: 2024-07-11
lastmod: 2024-07-11
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

I've spent some time optimizing [OpenSpartan Workshop](https://www.openspartan.com/docs/workshop/guides/get-started/) (the latest release is definitely mostly a performance improvement), and so you can now enjoy the fruits of my labor.

[**🚀 Download latest version**](https://github.com/OpenSpartan/openspartan-workshop/releases/download/1.0.7/OpenSpartan.Workshop.Installer.Bundle.exe)

![Animated GIF showing ranked match data in match overviews](images/blog/openspartan-workshop-latest-release-cylix/openspartan-workshop-latest.gif)

There are a few improvements here that are worth mentioning:

- [[#30](https://github.com/OpenSpartan/openspartan-workshop/issues/30)] Prevent auth loops if incorrect release number is used.
- [[#38](https://github.com/OpenSpartan/openspartan-workshop/issues/38)] Battlepass data now correctly renders on smaller screens, allowing scrolling.
- [[#39](https://github.com/OpenSpartan/openspartan-workshop/issues/39)] Removes the odd cross-out line in the calendar view.
- [[#41](https://github.com/OpenSpartan/openspartan-workshop/issues/41)] Fixes average life positioning, ensuring that it can't cause overflow.
- [[#43](https://github.com/OpenSpartan/openspartan-workshop/issues/43)] Fixed the issue where the medals are not rendered on first cold boot.
- [[#45](https://github.com/OpenSpartan/openspartan-workshop/issues/45)] Fixed an issue where refreshing The Exchange with no internet will result in an infinite spinning loading indicator.
- Improved image fallback for the **Exchange**, so that missing items now render properly.
- Season calendar now includes background images for each event, operation, and battle pass season. That will make it easier to spot which season starts when!
- Calendar colors are now easier to read.
- Fixes how ranked match percentage is calculated, now showing proper values for next level. You won't need to see an oddly 
- Home page now can be scrolled on smaller screens.
- Inside match metadata, medals and ranked counterfactuals correctly flow when screen is resized. This was a bit of a problem with smaller devices (e.g., older Microsoft Surface).
- The app now correctly reacts at startup to an error with authentication token acquisition. A message is shown if that is not possible.
- General performance optimizations and maintainability cleanup - OpenSpartan Workshop now starts and aggregates data quicker.
- Fixed an issue where duplicate ranked playlist may render in the Ranked Progression view.
- Fixed an issue where duplicate items may render in the **Exchange** view.
- Massive speed up to the **Operations** view load times - the app no longer issues unnecessary REST API calls to get currency data.

Refer to [getting started guide](https://www.openspartan.com/docs/workshop/guides/get-started/) to start using OpenSpartan Workshop! It's only a few clicks away.

## Feedback and improvements

That's about it for this release! [Go to the docs](/docs/workshop/guides/get-started/) to get started and download the latest version. If you have any feedback, such as bug reports or feature requests, [open an issue](https://github.com/OpenSpartan/openspartan-workshop/issues)!
