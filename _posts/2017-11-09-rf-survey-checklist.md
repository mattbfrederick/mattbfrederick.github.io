---
layout: post
title: "RF Survey Checklist"
date: 2017-11-09 06:58:33 -0600
permalink: /2017/11/09/rf-survey-checklist/
description: "I have been working on a project where we have to do surveys remotely with an on-site technician doing the actual surveying. They then send the survey file to one of our remote WLAN engineers and they review the file, make changes to the network, and then have the technician do another pass. They repeat…"
tags: ["aruba", "consulting", "ekahau", "ESS", "mobility", "networking", "remote", "rf survey", "Survey", "wifi", "wireless", "wlan"]
---

I have been working on a project where we have to do surveys remotely with an on-site technician doing the actual surveying. They then send the survey file to one of our remote WLAN engineers and they review the file, make changes to the network, and then have the technician do another pass. They repeat this process until the sites are tuned to the best of their abilities. Most of these sites are pre-existing designs that just got swapped for newer technology and the customer wants to verify that the wireless is as good as it can be.

This is for [Ekahau](https://www.ekahau.com/) and [Aruba Networking](http://www.arubanetworks.com/) products. I have also already created a training video for the on-site technicians on how to use and configure ESS, as well as the survey process. This is basically a checklist for the remote WLAN engineer that is reviewing the survey and making decisions. I had already developed this for the project, so I thought I might as well modify it and share it with the rest of the WLAN community for anyone else that may find themselves in a similar situation.

First is a flowchart for quick reference. Then below it is a more detailed overview of the items. Enjoy!

------------------------------------------------------------------------

![BLOG-RF Tuning Process](/assets/images/2017/11/blog-rf-tuning-process.png)

------------------------------------------------------------------------

**RF Survey Checklist**

1.  Have onsite technician send you a screen capture of how their channels are configured

![BLOG-RF Tuning Process2](/assets/images/2017/11/blog-rf-tuning-process2-e1510232144340.png)![BLOG-RF Tuning Process3](/assets/images/2017/11/blog-rf-tuning-process3.png)

2\. Verify with technician that they are on the latest version of Ekahau

3\. Verify all APs are up and functioning

4\. Verify AP groups that APs are currently in and change the power in the ARM profiles to the following: \*\*\*pre-determined EIRP values\*\*\*

5\. Set all ARM profiles to maintain

6\. Have technician start first run

7\. Once survey file is received, set ARM profiles back to single band

8\. Check that survey paths are not too far apart by setting Options \> Signal Prediction: Off and verifying that there are no holes between survey paths

9\. Double check channel configuration and \# of NICs using RTFM and Survey tab

![BLOG-RF Tuning Process4](/assets/images/2017/11/blog-rf-tuning-process4-e1510232172289.png)![BLOG-RF Tuning Process5](/assets/images/2017/11/blog-rf-tuning-process5-e1510232202933.png)

You can see the recorded information to verify that the correct channels were confirmed per NIC. Big red flags is seeing whole UNII bands missing or missing a radio in the Access Points tab on the survey. Double check what they are seeing in this view vs the “show ap active” from the controller to make sure we are seeing SSIDs on the channels the APs are broadcasting on. If the channel configuration was not correct, the onsite resource HAS to re-walk the site.
{: .indent-1}

10\. Remove survey paths from view

![BLOG-RF Tuning Process6](/assets/images/2017/11/blog-rf-tuning-process6-e1510231387950.png)

11\. Filter signal strength view to only include your APs

![BLOG-RF Tuning Process7](/assets/images/2017/11/blog-rf-tuning-process7.png)

12\. Start identifying coverage gaps and moving APs to the next power group as needed.

Main 3 groups:
{: .indent-1}

Low-power
{: .indent-2}

a xx dBm
{: .indent-3}

g xx dBm
{: .indent-3}

Med-power
{: .indent-2}

a xx dBm
{: .indent-3}

g xx dBm
{: .indent-3}

High-power
{: .indent-2}

a xx dBm
{: .indent-3}

g xx dBm
{: .indent-3}

Groups can have custom power levels if needed. Choose an unused AP group that you want those APs in that need non-standard power levels and edit its ARM profile. Example:
{: .indent-1}

```
configure terminal
rf arm-profile "New-power-g"
  max-tx-power xx
  min-tx-power xx
```

Try to keep similar power levels for all APs on a floor – avoid situations where you have some APs transmitting at higher power levels (\> or = 15 dBm EIRP) and some other transmitting at lower power levels (\< or = 6 dBm) on the same floor
{: .indent-1}

13\. 5 GHz is using 40 MHz channels by default, based on regulatory domain + density of APs, determine whether it needs to be moved to 20 MHz. This is edited in the ARM profiles with the command “40MHz-allowed-bands-none” and needs to be done on all “a” ARM profiles.

14\. Need to try achieve at least xx signal strength and xx SNR on both bands. Use best judgement on (1) How to achieve this (2) If it is plausible to achieve this (3) If it makes sense to achieve this – particularly on 2.4, you want to balance CCI with SNR/RSSI requirements.

15\. If seeing an excessive amount of 2.4 coverage, consider turning off 2.4 radios. This outlines a good process:

<https://www.ekahau.com/blog/2016/09/27/guest-blog-using-ekahau-site-survey-to-create-a-2-4-ghz-radio-plan/>Once last change is made, wait 10 minutes, set ARM back to maintain, and clear technician for another run.
{: .indent-1}

16\. Repeat process until surveying is complete

17\. Document any outstanding issues. Examples of issues:

1.  Multiple adjacent APs on the same channel
2.  High co-channel interference
3.  Large coverage areas for single APs
4.  Areas that don’t meet required signal strength and/or SNR
5.  Walls with higher-than-expected attenuation

18\. Once complete, take ARM out of maintain

19\. Name all surveys as \[SITECODE\]\_Survey_v1, v2, etc with last file having a \_Final
