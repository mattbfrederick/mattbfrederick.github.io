---
layout: post
title: "Indoor Arena Design Survey Part 2: The Concourse"
date: 2016-01-17 15:56:39 -0600
permalink: /2016/01/17/indoor-arena-design-survey-part-2-the-concourse/
description: "This is a continuation of Part 1 in a 3 part series on doing a design survey for an indoor arena. This part covers designing for the concourse. The concourse has two main areas of concern: general public and ticketing. Both present unique challenges and things to look out for. Ticketing Ticketing areas are definitely the most…"
tags: ["arena", "capacity", "high density", "large public venue", "networking", "wifi", "wireless"]
---

This is a continuation of [Part 1](/2016/01/11/indoor-arena-design-survey-part-1-the-bowl/) in a 3 part series on doing a design survey for an indoor arena. This part covers designing for the concourse.

The concourse has two main areas of concern: general public and ticketing. Both present unique challenges and things to look out for.

## Ticketing

Ticketing areas are definitely the most important part of the concourse. Offline ticket scans are a hot button with arena management as it messes with their numbers and reports. Therefore, it is normally a good idea to separate the ticketing design from everything else.

The first order of business is to get with the box office personnel and identify all areas where ticket scanners are used. It is important to either walk it or go over all of the maps with them. The main entrances are not always the only ticketing areas. There are VIP and Press areas that sometimes require tickets to be scanned as well. So, make sure you have every area identified prior to figuring out a coverage solution.

The next piece you need to figure out is the scanners themselves. See if you can get your hands on one to get the make and model. Then you will want to follow up with whoever configures the scanners. In most venues, the box office personnel do not configure the scanners. For example, in TicketMaster venues the TicketMaster technicians are the only ones that have the password to get into the configuration menu. It is important to follow up with them because even though you know the make and model, you still won’t know it’s capabilities until you talk to them or get into the configuration menu yourself. As an example, the Janam XM66 scanners are EXTREMELY popular, but there are several different options when ordering them. I have seen XM66’s that are dual band, some that are 2.4 GHz only, some that are dual band that support DFS channels, and some that don’t support DFS channels.

If possible, when doing your APOAS survey, bring one of the scanners with you to get it’s RSSI reading as well (they normally display in the top or in a status menu). This is a good idea because normally the scanners are held in a hand with a body in between the scanner and AP and it will show a much different reading than your laptop with an unobstructed external NIC. You want to be seeing a -65 dBm RSSI or better on the scanner during the survey.

The goal when designing for ticketing areas is to place the AP where you will get the most amount of coverage in ticketing areas while still maintaining a small overall cell size. The AP you will be deploying will exclusively for ticketing, so just worry about the scanners when planning coverage. The only time the AP will be dual purpose is if you have 2.4 GHz only scanners and want to put the public SSID on the 5 GHz radio.

So, to recap:

1.  Identify ALL areas where ticket scanners are used
2.  Identify and research the scanners and their capabilities
3.  Perform survey using scanners as well as surveying software
4.  Plan AP placement focusing on scanner coverage only

## General Public

The name of the game when designing coverage in the concourse is the keep it out of the bowl. The best way to achieve this is using directional antennas on the inside walls facing away from the bowl (omnidirectional antennas are normally not a good idea because it is hard to control the RF). There are a couple of reasons for this. First, you do not want clients in the bowl connecting to APs in the concourse. If that happens then the client will undoubtedly not have line of sight to the antenna and have to rely solely on multipath to get back to the AP. This causes massive amounts of retries, making the client bring down the overall performance of the channel. Second, you want roaming to as easy as possible to avoid the dreaded “sticky clients”. If a client moves from the concession stand to their seat by the one of the entrances, you don’t want that client still seeing the concourse AP at an “acceptable” RSSI (acceptable to the client and it’s roaming algorithm). Lastly, your bowl is already going to be jam packed with APs on the same channel – adding another just won’t help things.

[![concourse](/assets/images/2016/01/concourse-706w.png){: .aligncenter}](/assets/images/2016/01/concourse.png)

When looking around for potential AP locations, keep in mind that running cables in an arena concourse sucks. There is no false ceiling and trying to find pathways out of the public view is not fun. Also, IDF locations are normally sparse. There are not many candidates for dropping an access switch. So, try to look for places that already have network equipment. Things to keep an eye out for are things like IP cameras, existing APs that you will be replacing, and cable drops. The less you have to figure out on the implementation side the better.

[![concourse3](/assets/images/2016/01/concourse3-796w.png){: .aligncenter}](/assets/images/2016/01/concourse3.png)

When walking around, note areas that are likely to have a high concentration of people at a certain time. Main entrances are a great example of this. Prior to an event starting, people normally start forming up at the main entrances before doors open. This is also a time when people are standing around, not doing much, and more likely to be checking out things like the WiFi. Concession stands are also main congregation points. However, you normally don’t have to worry about high usage at concession stands since people normally are not on their phone for that long. For every person pulling out their phone, there is normally another person putting theirs away to order.

[![concourse4](/assets/images/2016/01/concourse4.png){: .aligncenter width="458"}](/assets/images/2016/01/concourse4.png)

Lastly, know your client base. Figure out what sort of events the venue normally hosts. This allows you to set expectations with yourself and the customer. High school events are more social gatherings than anything. I have been to high school games with 20k+ people attending and no more than 500 people on the wireless. When you start walking around you see why – everyone in the concourse is huddled in groups socializing and in the stands are mostly parents paying close attention to the game. You still see this at college sports events, just to a lesser extent. People still arrive and hang out in groups, but there is much more of a general fan base that is there with a small group of people they know. Pro sports events are even more segregated and thus more likely to have high connection rates.

To recap:

1.  Limit visibility of concourse APs in the bowl
2.  Plan for cabling
3.  Note areas needing more capacity
4.  Know your venue type

Stay tuned for Part 3: Back of House

Questions or comments? Leave them below and/or find me on twitter [@mattbfrederick](https://twitter.com/mattbfrederick)
