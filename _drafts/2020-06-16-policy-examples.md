---
Layout: post
title: 'Policy Examples'
description: 'examples of rules for policies'
date: '2020-06-10 00:00Z'
categories: post
---

# Policy Examples

## Network rule

| id | priority | precedence | match | action | exit |
|---|---|---|---|---|---|
| 1c59530e-ab5f-11ea-a616-fb7ea4ad304b | 100 | 4000 | under-attack=true | config.silverline=true | optional |
| 1c59530e-ab5f-11ea-a616-fb7ea4ad304b | 100 | 4000 | [under-attack=false AND config.silverline=true] | config.silverline=false | optional |
| 2fdd6648-ab57-11ea-8ccb-e3e79e8da2fd | 200 | 4000 | [ip.proto=6 AND vlan.id=1000 AND src.ip=10.10.1.0/24 AND dst.ip=10.10.1.245/32 AND dst.port=443] | accept | sufficient |
| 7211454c-ab58-11ea-a67b-d7f176529b83 | 200 | 4000 | [vlan.id="internal" AND ip.version=6 AND src.ip=fd00:2ea9:2bc0:0100:/64] | translate.src.prefix=2001:db8:dead:beef:/64 | sufficient |

## session rule

| id | priority | precedence | match | action | exit |
|---|---|---|---|---|---|
| 0f9f7f8c-ab58-11ea-98db-d3131df2bd54 | 200 | 500 | subscriberID!="null" | accept | optional |
| edce664c-ab5d-11ea-8bcd-17c6b39e9fd1 | 200 | 500 | subscriberID=watchlist.fraud | mirror.fraud | optional |
| 1910c552-ab5e-11ea-88ae-ab509642015c | 200 | 500 | subscriberID=watchlist.legal | mirror.legal | optional |

## application rule

| id | priority | precedence | match | action | exit |
|---|---|---|---|---|---|
| 2fdd6648-ab57-11ea-8ccb-e3e79e8da2fd | 100 | 70 | [src.ip.whereis=nigeria AND ip.src.reputation > 5] | chain.honeynet | requisite |
| 981de068-ab64-11ea-a1c9-1f96679db721 | 300 | 70 | classification=video | [chain.video AND shape.policy=SDvideo] | optional |
| 8c342f0a-ab64-11ea-8af0-3f44713a486b | 300 | 70 | classification=gaming | chain.gaming | optional |
| 82884482-ab64-11ea-b9d1-2bb11e28f356 | 300 | 70 | classification=dns-over-http | nexthop.application="ns.example.com" | optional |

## composed policy

| id | priority | precedence | match | action | exit |
|---|---|---|---|---|---|
| 1c59530e-ab5f-11ea-a616-fb7ea4ad304b | 100 | 4000 | under-attack=true | config.silverline=true | optional |
| 1c59530e-ab5f-11ea-a616-fb7ea4ad304b | 100 | 4000 | [under-attack=false AND config.silverline=true] | config.silverline=false | optional |
| 2fdd6648-ab57-11ea-8ccb-e3e79e8da2fd | 100 | 70 | [src.ip.whereis=nigeria AND ip.src.reputation > 5] | chain.honeynet | requisite |
| 2fdd6648-ab57-11ea-8ccb-e3e79e8da2fd | 200 | 4000 | [ip.proto=6 AND vlan.id=1000 AND src.ip=10.10.1.0/24 AND dst.ip=10.10.1.245/32 AND dst.port=443] | accept | sufficient |
| 7211454c-ab58-11ea-a67b-d7f176529b83 | 200 | 4000 | [vlan.id="internal" AND ip.version=6 AND src.ip=fd00:2ea9:2bc0:0100:/64] | translate.src.prefix=2001:db8:dead:beef:/64 | sufficient |
| 0f9f7f8c-ab58-11ea-98db-d3131df2bd54 | 200 | 500 | subscriberID!="null" | accept | optional |
| edce664c-ab5d-11ea-8bcd-17c6b39e9fd1 | 200 | 500 | subscriberID=watchlist.fraud | mirror.fraud | optional |
| 1910c552-ab5e-11ea-88ae-ab509642015c | 200 | 500 | subscriberID=watchlist.legal | mirror.legal | optional |
| 981de068-ab64-11ea-a1c9-1f96679db721 | 300 | 70 | classification=video | [chain.video AND shape.policy=SDvideo] | optional |
| 8c342f0a-ab64-11ea-8af0-3f44713a486b | 300 | 70 | classification=gaming | chain.gaming | optional |
| 82884482-ab64-11ea-b9d1-2bb11e28f356 | 300 | 70 | classification=dns-over-http | nexthop.application="ns.example.com" | optional |
