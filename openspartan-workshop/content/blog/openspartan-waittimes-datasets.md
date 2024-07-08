---
title: "Track Halo Infinite wait times with open source datasets"
slug: openspartan-waittimes-datasets
description: "Get a glimpse into the aggregated playlist wait times for Halo Infinite."
summary: "Get a glimpse into the aggregated playlist wait times for Halo Infinite."
date: 2024-07-08
lastmod: 2024-07-08
draft: false
weight: 50
categories: [Data]
tags: [data,open-source,halo-infinite]
contributors: [Den Delimarsky]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Starting today, you can access [OpenSpartan Wait Time Datasets](https://github.com/OpenSpartan/waittimes-datasets) - an open-source effort to aggregate and track playlist wait times for Halo Infinite. 

{{< link-card
  title="Look at the data"
  description="Explore the currently captured wait times."
  href="https://github.com/OpenSpartan/waittimes-datasets"
  target="_blank"
>}}

Starting with [Banished Honor](https://www.halowaypoint.com/news/banished-honor-operation-launch) and [Tenrai IV](https://www.halowaypoint.com/news/tenrai-iv-operation-launch), and continuing with future operations, this collection will contain snapshots of playlist wait times in 10 minute increments, allowing anyone to easily see the changes in wait times over the span of an operation.

{{< figure class="rounded-3 h-auto" src="images/blog/openspartan-waittimes-datasets/openspartan-waittimes-github.png" alt="Screenshot of the OpenSpartan Wait Time Datasets repository on GitHub." caption="Screenshot of the OpenSpartan Wait Time Datasets repository on GitHub." >}}

The GitHub repository contains data in the [`datasets`](https://github.com/OpenSpartan/waittimes-datasets/tree/main/datasets) folder, where you can find SQLite databases for each operation for which wait time data was captured. Because the data is captured in an open format, you can use a myriad of tools to query and render it, such as [Jupyter notebooks](https://jupyter.org/) with [Python](https://www.python.org/) and even dedicated tools like [DB Browser for SQLite](https://sqlitebrowser.org/).

![Event metadata shown in OpenSpartan Workshop](images/blog/openspartan-waittimes-datasets/db-browser-sqlite.gif)

Every SQLite database is small enough that it can be queried on even the most low-spec PCs or cloud workloads.

There are two key tables that are of interest:

| Table | Description |
|:------|:------------|
| `PlaylistMetadata`  | Outlines all playlist-related metadata, including playlist asset ID, version ID, name, description, and more. |
| `WaitTimeSnapshots` | Captures playlist wait times, in 10 minute intervals. Data includes snapshot timestamp (PT time zone), asset ID, version ID, and wait time in seconds. |

While for now the data is captured only for the **US west coast**, I am looking at expanding that in the future to other time zones and regions.

## FAQ

### I see that some data is missing in the Banished Honor and Tenrai IV datasets. What's up with that?

During the data capture I was still stabilizing the tool against issues that resulted in data misses. That resulted in some gaps. Moving forward (starting with the Anvil operation), those gaps should be minimal (if any) and contain the full spectrum of data in 10 minute intervals.

### Can you share the tool that captured this data?

Planning on doing this in the future. For the time being, you can familiarize yourself with the process [on my blog](https://den.dev/blog/halo-infinite-playlist-wait-time-api/).

### Can I use this data to render wait time graphs?

Yes. What I have here is the raw data - you can use it to slice and dice the metrics as you see fit.

### Data is captured for the US west coast. Do you have plans for capturing metrics for other time zones or regions?

I am currently looking at the best ways to capture this data for other regions, but if the changes are going to be happening, it will be _after_ Anvil. The Anvil data set will contain only PT data captures.

## Other tools you should check out

- [OpenSpartan Workshop](https://openspartan.com/docs/workshop/guides/get-started/)
- [OpenSpartan XUID Resolver](https://github.com/OpenSpartan/xuid-resolver)
- [OpenSpartan Career Rank Extractor](https://github.com/OpenSpartan/career)

To see the latest on supported tools and data, refer to the [official OpenSpartan site](https://openspartan.com) and [my blog](https://den.dev/tags/halo-api/).
