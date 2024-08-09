---
title: "Purchasing Items In The Halo Infinite Exchange Via The API"
slug: purchasing-items-halo-infinite-exchange-api
description: "Let's talk a bit more about the Exchange. I've discussed its API implementation on my blog when it first came out in retail builds of Halo Infinite, but now that I am growing the content on the OpenSpartan website, I decided to start documenting my API explorations here. After all, it's all at home with the rest of the Halo-related tinkering that I am doing."
summary: "How to programmatically perform Exchange transactions with the Halo Infinite API"
date: 2024-08-08
lastmod: 2024-08-08
weight: 50
categories: [Halo API]
tags: [halo-infinite,halo-api,halo-infinite-api]
contributors: [Den Delimarsky]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

Let's talk a bit more about the Exchange. I've discussed its API implementation [on my blog](https://den.dev/blog/halo-infinite-exchange-spartan-points/) when it first came out in retail builds of Halo Infinite, but now that I am growing the content on the OpenSpartan website, I decided to start documenting my API explorations here. After all, it's all at home with the rest of the Halo-related tinkering that I am doing.

## Getting existing offerings and their prices

{{< callout context="tip" title="Protected APIs" icon="info-circle" >}}
All API requests below require authentication. Refer to [my blog post](https://den.dev/blog/halo-api-authentication/) to learn how it can be automated.
{{< /callout >}}

As you might've seen from the aforementioned blog post, the Exchange is nothing but a facet of the Halo Infinite store. You can call a static endpoint to see the currently active offers:

```bash
https://economy.svc.halowaypoint.com
  /hi
  /players
  /xuid(YOUR_XUID)
  /stores
  /softcurrencyoffers
  ?flight=YOUR_CLEARANCE
```

{{< callout context="tip" title="What is soft currency?" icon="info-circle" >}}
In the endpoint above you might've noticed the term `softcurrency` - unlike currency that is obtained by _paying real money_, soft currency is nothing other than Spartan Points that you earn in the game. The endpoint we're querying returns the offerings that are available in exchange for Spartan Points (i.e., soft currency).
{{< /callout >}}

This, in turn, will return the _currently active_ offerings in the Exchange, like this (notice the highlighted costs):

```json {hl_lines=[15,45]}
{
    "StoreId": "SoftCurrencyOffers",
    "StorefrontExpirationDate": {
        "ISO8601Date": "2024-09-03T17:00:00Z"
    },
    "StorefrontDisplayPath": "StoreContent/Display/Storefront/Exchange-20240429-00.json",
    "Offerings": [
        {
            "OfferingId": "20230210-01",
            "OfferingDisplayPath": "StoreContent/Display/Offerings/20230210-01.json",
            "OfferingExpirationDate": null,
            "IncludedItems": [],
            "Prices": [
                {
                    "Cost": 1000,
                    "CurrencyPath": "Currency/Currencies/cR.json"
                }
            ],
            "IncludedCurrencies": [],
            "IncludedRewardTracks": [],
            "BoostPath": null,
            "OperationXp": 0,
            "EventXp": 0,
            "MatchBoosts": null,
            "RewardTrackAdjustments": []
        },
        {
            "OfferingId": "20240429-21",
            "OfferingDisplayPath": "StoreContent/Display/Offerings/20240429-21.json",
            "OfferingExpirationDate": null,
            "IncludedItems": [
                {
                    "Amount": 1,
                    "ItemPath": "Inventory/Armor/Helmets/3839789-ArmorHelmet-Mark-V-B.json",
                    "ItemType": "ArmorHelmet"
                },
                {
                    "Amount": 1,
                    "ItemPath": "Inventory/Armor/HelmetAttachments/3839790-ArmorHelmetAttachment-Mark-V-B.json",
                    "ItemType": "ArmorHelmetAttachment"
                }
            ],
            "Prices": [
                {
                    "Cost": 75000,
                    "CurrencyPath": "Currency/Currencies/softcurrency.json"
                }
            ],
            "IncludedCurrencies": [],
            "IncludedRewardTracks": [],
            "BoostPath": null,
            "OperationXp": 0,
            "EventXp": 0,
            "MatchBoosts": null,
            "RewardTrackAdjustments": []
        },
        [...MORE OFFERINGS HERE...]
```

We can skip the _very first offering_ in the list because it's a pointer back to the store (that's how linking works in the Store infrastructure), but jumping to the next one we see something for **75,000 Spartan Points**. If we look inside the game, there is only one item that costs this much _at this point in time_ - the **Ghosts of Reach helmet**:

{{< figure class="rounded-3 h-auto" src="images/blog/purchasing-items-halo-infinite-exchange-api/ghosts-of-reach.png" alt="Ghosts of Reach helmet as seen in the Halo Infinite in-game Exchange." caption="Ghosts of Reach helmet as seen in the Halo Infinite in-game Exchange." >}}

## Purchasing items

Similarly, if we would scroll through the returned JSON we would spot other items available on the Exchange. But what we're really after here is _purchasing these items through the API_ - that is, if I want to update [OpenSpartan Workshop](https://openspartan.com/docs/workshop/guides/get-started/) to [support Exchange purchases](https://github.com/OpenSpartan/openspartan-workshop/issues/34) then I need to make sure that I find out more about the API that is needed here.

A bit of tinkering and investigation with [the help of the trusty Fiddler tool](https://den.dev/blog/halo-api/), and the API endpoint is uncovered - we `POST` to the following:

```bash
https://economy.svc.halowaypoint.com
  /hi
  /players
  /xuid(YOUR_XUID)
  /storetransactions
```

The body of the request is as follows:

```json
{
    "StoreId": "SoftCurrencyOffers",
    "ExpectedOfferingPrice":
    {
        "Cost": 750,
        "CurrencyPath": "Currency/Currencies/softcurrency.json"
    },
    "OfferingId": "20240429-01",
    "TrackingId": "A_TRACKING_GUID"
}
```

Based on what we saw above, we should have all the necessary components to make this happen - we have the `OfferingId`, we know how much it costs so that we can fill out `ExpectedOfferingPrices`, and of course we have a reference to the soft currency path.

But before we do any of this, wouldn't it be logical for us to _check the Spartan Points balance_? It would be, so let's start there first. I want to make sure that OpenSpartan Workshop doesn't issue any requests unless it has to, and we can only start purchasing items if there is enough balance available. To do that, we issue a request to the following `economy` endpoint:

```bash
https://economy.svc.halowaypoint.com
  /hi
  /players
  /xuid(YOUR_XUID)
  /currencies
  /softcurrency
```

The response we get is this:

```json
{
    "Amount": 79300,
    "CurrencyPath": "Currency/Currencies/softcurrency.json"
}
```

Looks accurate, I indeed have 79,300 Spartan Points, as seen in the screenshot above.

{{< callout context="tip" title="Can I look up Spartan Points balances for other players?" icon="info-circle" >}}
Even if you know another player's XUID, you won't be able to look up their currency balances - if you issue a request to the endpoint with a XUID that is different than the one that is associated with your Spartan Token, you will get a `403 Forbidden` response.
{{< /callout >}}

Back to trying to purchase items from the Exchange - now that we can verify the balance, let's issue a `POST` request to the store transactions endpoint and see what happens. But maybe, just maybe - we also try to tinker with the _cost of the item_. If it's bundled in the request, does it mean that we can get an item cheaper than what is listed in the Exchange? For the **Ghosts of Reach helmet**, we craft the following JSON body:

```json
{
    "StoreId": "SoftCurrencyOffers",
    "ExpectedOfferingPrice":
    {
        "Cost": 0,
        "CurrencyPath": "Currency/Currencies/softcurrency.json"
    },
    "OfferingId": "20240429-21",
    "TrackingId": "A_NEWLY_GENERATED_GUID"
}
```

{{< callout context="tip" title="Transaction tracking ID" icon="info-circle" >}}
The `TrackingId` property can be see to an arbitrary GUID that you generate yourself. It doesn't need to be associated with any other items - it will uniquely identify the receipt of the transaction upon completion.
{{< /callout >}}

Wouldn't it be nice if we got it for free, right? As it turns out (and, unsurprisingly) there is also a server-side check to make sure that the price is correct - we're getting a `409 Conflict` response with:

```json
{
    "Error": "UnexpectedOfferingPrice"
}
```

D'oh! But that's OK. Let's update the price to be the real price - **75,000 Spartan Points**. The body of the request is now this:

```json
{
    "StoreId": "SoftCurrencyOffers",
    "ExpectedOfferingPrice":
    {
        "Cost": 75000,
        "CurrencyPath": "Currency/Currencies/softcurrency.json"
    },
    "OfferingId": "20240429-21",
    "TrackingId": "MY_TRANSACTION_GUID"
}
```

And once we execute the request, we get a predictable response:

```json
{
    "Receipt": {
        "PricePaid": {
            "Cost": 75000,
            "CurrencyPath": "Currency/Currencies/softcurrency.json"
        },
        "TransactionId": "MY_TRANSACTION_GUID",
        "TransactionDate": {
            "ISO8601Date": "2024-08-09T01:02:01.903Z"
        },
        "UpdatedRewardTracks": [],
        "GrantedItems": [
            {
                "ItemPath": "Inventory/Armor/Helmets/3839789-ArmorHelmet-Mark-V-B.json",
                "Amount": 1,
                "Source": "Store"
            },
            {
                "ItemPath": "Inventory/Armor/HelmetAttachments/3839790-ArmorHelmetAttachment-Mark-V-B.json",
                "Amount": 1,
                "Source": "Store"
            }
        ],
        "GrantedCurrencies": [],
        "ActivatedBoosts": []
    },
    "PlayerState": {
        "RewardTracks": [],
        "ItemBalances": [
            {
                "Amount": 1,
                "ItemId": "3839789-005-reach",
                "ItemPath": "Inventory/Armor/Helmets/3839789-ArmorHelmet-Mark-V-B.json",
                "ItemType": "ArmorHelmet",
                "FirstAcquiredDate": {
                    "ISO8601Date": "2024-08-09T01:02:01.962Z"
                }
            },
            {
                "Amount": 1,
                "ItemId": "3839790-004-reach",
                "ItemPath": "Inventory/Armor/HelmetAttachments/3839790-ArmorHelmetAttachment-Mark-V-B.json",
                "ItemType": "ArmorHelmetAttachment",
                "FirstAcquiredDate": {
                    "ISO8601Date": "2024-08-09T01:02:01.97Z"
                }
            }
        ],
        "CurrencyBalances": [
            {
                "Amount": 4300,
                "CurrencyPath": "Currency/Currencies/softcurrency.json"
            }
        ],
        "RefreshNeeded": false,
        "Boosts": []
    }
}
```

Seems like things went through just fine. As a bonus, notice the `CurrencyBalances` property - the API returns the remaining Spartan Points balance so that I don't need to issue yet another `GET` request to update any display elements that render the current balance.

What's neat about the implementation of this API is that even if you re-send the request (with the same transaction ID), you will still get the same receipt - that's the signal that you already own the item and won't be charged for it again, even if the pricing in the body is correct. If the transaction ID is different, you will get a `409 Conflict` response with the following body:

```json
{
    "Error": "ItemsAlreadyOwned"
}
```

Now, launching the game and looking inside the armor hall, I can see that that helmet has been added to my inventory:

{{< figure class="rounded-3 h-auto" src="images/blog/purchasing-items-halo-infinite-exchange-api/ghost-helmet.png" alt="Ghosts of Reach helmet as seen in the Halo Infinite armor hall." caption="Ghosts of Reach helmet as seen in the Halo Infinite armor hall." >}}

## Conclusion

Simple as that! You now know how you can programmatically purchase items from the Exchange. This functionality will soon be coming to OpenSpartan Workshop, so stay tuned for future updates.
