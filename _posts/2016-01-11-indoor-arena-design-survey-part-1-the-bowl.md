---
layout: post
title: "Indoor Arena Design Survey – Part 1: The Bowl"
date: 2016-01-11 02:47:29 -0600
permalink: /2016/01/11/indoor-arena-design-survey-part-1-the-bowl/
description: "This post should be used as a quick reference for anyone needing to do a design and site survey for an indoor arena. This was written for WLAN engineers that already have experience in designing and implementing WiFi. This is part 1 of 3 for performing a design focused site survey on an arena. The…"
tags: ["arena", "capacity", "high density", "large public venue", "networking", "wifi", "wireless"]
---

This post should be used as a quick reference for anyone needing to do a design and site survey for an indoor arena. This was written for WLAN engineers that already have experience in designing and implementing WiFi.

This is part 1 of 3 for performing a design focused site survey on an arena. The three parts will be broken out into:

1.  Bowl
2.  Concourse
3.  Back of house

I highly recommend reading through [Chuck Lukaszewski’s Validated Reference Designs](http://community.arubanetworks.com/t5/Validated-Reference-Design/Very-High-Density-802-11ac-Networks-Validated-Reference-Design/ta-p/230891) on Very High Density networks. He goes into much deeper detail than I could ever hope to with this. This is hoping to provide the tools to gather the information you will need. Most of the information in here is meant to be used in conjunction with those documents to develop your design.

## Now….onto the Bowl…

The bowl is the most important part in an arena design. The bowl is where the majority of clients connect and spend most of their time and it is the one area pretty much guaranteed to break a poorly designed network. Here is my process for a survey of the bowl:

1.  Figure out your mounting options
2.  Figure out your capacity requirements
3.  Find your “gotchas”
4.  Build your survey kit
5.  Setup your survey kit
6.  Survey and analyze
7.  Plan your coverage

The order of the first three doesn’t really matter as you do all of them at pretty much the same time on your first initial walk-through.

## Figure out your mounting options

Your first task is to figure out where you can actually mount your Access Points (APs). Arenas normally have a few different options that you can mount your APs and antennas on. Common mounting options are:

- A catwalk or rigging structure suspended in the middle
- Outer walls and overhangs
- Under the seats
- Under the concrete beneath the seats

Now, one of these is a great option, one is OK, and the other two are just a bad idea. Let’s take them one by one.

#### Catwalk or rigging structure

A catwalk or rigging structure suspended in the middle is by far your best option. This gives the clients direct line-of-sight (LoS) to the antennas of the AP. It also gives you a chance on achieving spatial reuse in the bowl (which is extremely hard to do and the smaller the bowl, the harder it is) and gives free space between the AP and clients.

*\*\*\*Side note: Spatial reuse and channel reuse are two different things. If you want a really good explanation on this, then google “Chuck Lukaszewski” and click on any one of his talks because I am pretty sure he mentions this in all of them. A quick explanation is “channel reuse” means you are reusing a channel – that’s it. “Spatial reuse” means you are actually reusing the spectrum and allowing two APs on the same channel to tx and rx at the same time by limiting how much the AP and clients that it is serving can hear the other AP.\*\*\**

[![inside out](/assets/images/2016/01/inside-out-600w.png)](/assets/images/2016/01/inside-out.png)

*By aiming your antennas away from each other, you decrease your CCI and increase your chance of achieving actual spatial reuse*
{: .caption style="max-width: 600px"}

[![Example of a cat walk](/assets/images/2016/01/cat-walk-600w.png)](/assets/images/2016/01/cat-walk.png)

*Example of a catwalk*
{: .caption style="max-width: 600px"}

Every indoor arena I have ever been to has had a catwalk. However, there are some that have even better options – like this lighting grid:

[![Lighting/rigging grid](/assets/images/2016/01/grid-600w.jpg)](/assets/images/2016/01/grid.jpg)

*Lighting/rigging grid*
{: .caption style="max-width: 600px"}

The lighting grid ends up being better than a catwalk because it increases the mounting area for your APs. This means you can have better AP spacing and mount in places that a catwalk would not normally allow.

As long as it’s a suspended structure in the middle of the arena that allows you to aim the antennas towards the outer walls instead of the middle – it is a decent option.

#### Outer walls and overhangs

Outer walls and overhangs are a bad idea in an indoor arena. You eliminate any chances you might have had at spatial reuse, while also drastically raising your CCI (Co-Channel Interference). Also, you are forcing clients to transmit behind the user which means it is no longer LoS.

[![An example of what not to do](/assets/images/2016/01/outside-in-600w.png)](/assets/images/2016/01/outside-in.png)

*An example of what not to do*
{: .caption style="max-width: 600px"}

You will see some outdoor stadiums with variations of this design and it almost always just due to lack of options.

#### Under the seats

Under the seats is an OK option in an indoor arena. It does keep your cell sizes down due to body attenuation, thus increasing your chances of spatial reuse. However, it does have some drawbacks. One is that your clients no longer have LoS to the antennas, which increases packet retries. It also in inevitably more expensive because you will most likely have to drill through concrete, run conduit, install secure enclosure, etc.

[![Example of "under the seat" deployment](/assets/images/2016/01/undet-the-seat.png){: width="600"}](/assets/images/2016/01/undet-the-seat.png)

*Example of “under the seat” deployment*
{: .caption style="max-width: 600px"}

This is normally the best option for outdoor arenas since you usually won’t have a permanent structure in the middle to mount to.

*Update September 2016 – I just wanted to add a quick note about under the seat deployments. They have performed extremely well in outdoor stadiums. The abilities it creates for spatial reuse makes it a front-runner in deployment types and the cost can be justified in a venue the size of a 30k+ outdoor stadium. As for indoor arenas, there has been some testing with this, but there hasn’t been enough evidence that the possibility of a little bit of improvement in spatial reuse in a 20k seat indoor arena would justify the extra cost (normally 2-3x more).*

#### Under the concrete

Under the concrete is a bad idea. Concrete introduces a much larger amount of attenuation in the 5 GHz band than in 2.4 GHz. This was a popular design 5+ years ago. However, after seeing how the RF behaves and the increasing urgency to get as many clients on the 5 GHz band as possible, it just doesn’t work out. Even with maxing the power on your 5 GHz radios and turning the 2.4 GHz radios down, it’s next to impossible to get any sort of decent band split. You are also now asking your clients to also transmit through people and concrete, increasing your packet retires.

[![Example of "under the concrete" mounting](/assets/images/2016/01/under-concrete.png)](/assets/images/2016/01/under-concrete.png)

*Example of “under the concrete” mounting*
{: .caption style="max-width: 426px"}

## Figuring out your capacity requirements

Once again, Chuck’s guides are fantastic at explaining how to size your network. For the sake of the survey, we just need to find out the minimum amount of APs we need in the bowl. Now, this will depend on which vendor you are deploying and you need to know the maximum clients their radio can handle. As a rule, you don’t want to have more than 150 clients per radio in the worst (or best…depending on how you choose to look at it) case scenario. So, if I had an arena with a maximum seating capacity of 9,000 people, I would need a minimum of 30 APs if I am deploying dual-radio APs (9,000/300=30). This would mean the network would still work, albeit limping, if every single person in the bowl was connected at the same time.

The reason for not overcompensating with APs is that there is a trade-off between AP capacity and CCI/ACI. Once you deploy over 24 APs in the bowl, each additional radio is potential CCI if you are unable to achieve your spatial reuse. So, I always try to deploy the minimum amount of APs that will still work if the max devices possible connected.

Now we know we need a minimum of 30 APs in the bowl.

## Finding your “gotchas”

Now, it’s time to start trying to find things that are out of the ordinary for this particular arena. This means walking the whole bowl. Here are some examples of some “gotchas”:

Open concourses – Normally, there is a narrow opening every 20-30′ between the concourse and bowl. This normally allows for good separation between the RF in the concourse and the bowl. Sometimes you will run into arenas that have their concourse or parts of their concourse wide open. This creates challenges when trying to keep clients in the bowl from connecting to APs meant for the concourse.

[![Open concourses](/assets/images/2016/01/gotcha1.png){: width="600"}](/assets/images/2016/01/gotcha1.png)

*Open concourses*
{: .caption style="max-width: 600px"}

LoS obstructions – It’s good to walk to all areas of the bowl because you might find some sections or rows that have obstructions between them and your potential APs. Some examples…

[![Top two rows obstructed by metal mesh. Also, metal mesh behind them will distort the signal.](/assets/images/2016/01/gotcha2-338w.jpg)](/assets/images/2016/01/gotcha2.jpg)

*Top two rows obstructed by metal mesh. Also, metal mesh around them will cause massive attenuation*
{: .caption style="max-width: 338px"}

[![Top rows of lower bowl shadowed by overhang](/assets/images/2016/01/gotcha4-600w.jpg)](/assets/images/2016/01/gotcha4.jpg)

*Top rows of lower bowl shadowed by overhang*
{: .caption style="max-width: 600px"}

[![I-beam ran alongside catwalk, creating an obstruction between the cat walk and lower seats](/assets/images/2016/01/gotcha3-600w.jpg)](/assets/images/2016/01/gotcha3.jpg)

*I-beam ran alongside catwalk, creating an obstruction between the catwalk and lower seats*
{: .caption style="max-width: 600px"}

Configurable seating – Be aware of what seating is permanent and what seating is configurable. I have been to some arenas that have 25+ seating configurations.

[![Configurable seating](/assets/images/2016/01/gotcha5-600w.jpg)](/assets/images/2016/01/gotcha5.jpg)

*Configurable seating*
{: .caption style="max-width: 600px"}

There are some that you can design plausible solutions for. However, there are some that simply will do more damage than good. If you can create a solution that doesn’t degrade the system as a whole, then add it to the design. However, doing something like adding several APs and raising your CCI just to cover a single row isn’t a good idea. If you can’t design a solution that will benefit the overall system then note it as a potential problem area in the design.

## Build your survey kit

I normally have the same process for every indoor arena. Once I have found a spot I can mount to on the catwalk, I go to a hardware store and get some PVC. I then setup a “T” with an Aruba IAP-224 (soon to be IAP-228), two diplexers (224 has dual band ports), ANT-5314, ANT-2314, RPSMA-M to NM and NM to NM jumpers, 802.3at PoE injector, power cord, and [portable battery](http://www.homedepot.com/p/BLACK-DECKER-500-Watt-Portable-Power-Station-PPRH5B/203613410?cm_mmc=Shopping%7cTHD%7cG%7c0%7cG-BASE-PLA%7c&gclid=Cj0KEQiAn8i0BRDur-HV1PCTy4UBEiQAPuFr9EvQGFzrWyRaj6H5gtCfc5s-rBNdxIdFzj5obVaQStEaAu2m8P8HAQ&gclsrc=aw.ds). I normally make it around 6′ tall to make sure I have enough room to fasten it to the rails of the catwalk.

[![Bowl survey kit](/assets/images/2016/01/survey-kit-338w.jpg)](/assets/images/2016/01/survey-kit.jpg)

*Bowl survey kit*
{: .caption style="max-width: 338px"}

Now, this is another case of having to know your vendor. I use Aruba IAPs. There are a couple of things you have to do to configure the IAP as a survey AP. You have to configure it to function as a Standalone AP and configure the SSID to work without a wired uplink. I will make a separate blog post on this. Whatever type of AP you are using, make sure it is configured and tested prior to hanging it. The catwalk is not a fun place to troubleshoot an AP.

## Setup your survey kit

Now that you have your survey kit built, it is time to hang it. To do this safely, it should be done with two people and during a time with no one on the arena floor. You lower the survey kit over the side where the antennas are ~2′ below the catwalk and fasten it to the rails in at least two places. You also want some sort of secondary fall protection on it separate from the main mounts in case they fail. Also have the second person stay on the catwalk while you survey.

*\*\*\*Do not skimp on this part or take it lightly. Make sure it is 100% secure and everyone is alert while doing this. The catwalk is very high up and this equipment is heavy. If you are surveying and see someone walking below the mount, ask them to walk around.\*\*\**

[![Survey kit installed on cat walk](/assets/images/2016/01/survey-kit-install-600w.jpg)](/assets/images/2016/01/survey-kit-install.jpg)

*Survey kit installed on catwalk*
{: .caption style="max-width: 600px"}

When you mount the antennas, make sure they are aimed at the middle of a large section. Also, make sure that section has sections on both sides.

When the AP is up, make sure that you have your power and channels statically assigned. I normally remove all channels besides one 5 GHz channel (64) and 2.4 GHz channel (11). I then set my tx power to 6 dBm (4 mW). I rarely end up having an AP deployed above this power level in the bowl.

## Survey

When performing the survey, I survey the main section and all of it’s bordering sections. I stop and save at the end of every section.

[![Survey path](/assets/images/2016/01/survey-path-600w.png)](/assets/images/2016/01/survey-path.png)

*Survey path*
{: .caption style="max-width: 600px"}

Once you are finished with the survey, filter the results to show only the survey SSID. Then bring your cell size down to a -55 dBm cutoff. I recommend planning for -55 dBm cell sizes in the empty stadium because you can normally count on around 10 dBm of attenuation when the arena is full.

[![Results filtered to survey SSID with -55 dBm cutoff](/assets/images/2016/01/survey-results-600w.png)](/assets/images/2016/01/survey-results.png)

*Results filtered to survey SSID with -55 dBm cutoff*
{: .caption style="max-width: 600px"}

## Plan your coverage

Now that we have our cell size, we can plan the coverage. This step is pretty simple, you pull up an arena map and create a cell that matches the same coverage as your survey results. Then replicate until you have all of your coverage areas filled in.

[![Coverage plan](/assets/images/2016/01/cc-bowl-aps-600w.png)](/assets/images/2016/01/cc-bowl-aps.png)

*Coverage plan*
{: .caption style="max-width: 600px"}

Make sure you account for scorer’s/judge’s tables and floor seats. Using the 9,000 seat arena example from earlier, I need a minimum of 30 APs. My coverage plan gives me a total of 44 to cover everything I need. If evenly distributed, then each AP will have roughly 200 clients if everyone in the bowl was connected at the same time.

Voila! Now we know what kind of APs and antennas we are using in the bowl, where we are mounting, and how many we need.

Stay tuned for Part 2: The Concourse

Questions or comments? Leave them below and/or find me on twitter [@mattbfrederick](https://twitter.com/mattbfrederick)
