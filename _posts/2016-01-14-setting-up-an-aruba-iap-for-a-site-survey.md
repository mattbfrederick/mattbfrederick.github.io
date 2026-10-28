---
layout: post
title: "Aruba: Setting up an Aruba IAP for a Site Survey"
date: 2016-01-14 22:27:58 -0600
permalink: /2016/01/14/setting-up-an-aruba-iap-for-a-site-survey/
description: "Note: This is pre-InstantOS 4.2 I used to do my site surveys with a lightweight AP and a small controller. I have since moved to IAPs. Once you get the initial setup done, you can just power it up and go at each survey point. Here are the steps: Connect a console cable to the…"
tags: ["aruba networks", "IAP", "Survey"]
---

*Note: This is pre-InstantOS 4.2*

I used to do my site surveys with a lightweight AP and a small controller. I have since moved to IAPs. Once you get the initial setup done, you can just power it up and go at each survey point. Here are the steps:

1.  Connect a console cable to the CONSOLE port of the IAP and connect to your laptop
2.  Open up your Telnet/SSH client and start a session on the necessary COM port
3.  Connect the Data In port of the injector to the ethernet port of your laptop
    1.  You do this because the the IAP will NOT broadcast the “Instant” SSID without an uplink in the beginning.
4.  Connect the Power + Data Out port of the injector to the ENET0 port of the IAP
5.  Once the AP starts booting, start pressing the Enter key over and over until you see “apboot\>”
    1.  You can wait until you see “Hit *\<*Enter*\>* to stop autoboot”, but you only have a couple of seconds once that shows up and it doesn’t hurt anything to start pressing Enter right away.
6.  You need to set a static address for the IAP to stay up (it needs either static or DHCP). Follow [these steps](https://community.arubanetworks.com/t5/Controller-less-WLANs/How-do-I-configure-a-static-IP-addreess-to-an-Instant-Access/ta-p/185590) to set a static address on the IAP
7.  Once the address is assigned, type “boot” and press Enter
8.  A few minutes after the IAP is fully booted, you will see the “Instant” SSID broadcasting. Connect the SSID and navigate to [https://instant.arubanetworks.com](https://instant.arubanetworks.com)
    1.  Default login is admin/admin
9.  Once in the GUI, click “Maintenance” in the top-right corner to convert the IAP to Standalone mode (This configures the IAP to not look for other IAPs/Virtual Controllers):
    1.  [![convert](/assets/images/2016/01/convert.png){: width="300"}](/assets/images/2016/01/convert.png)
    2.  [![standalone](/assets/images/2016/01/standalone.png){: width="300"}](/assets/images/2016/01/standalone.png)
10. After converting to Standalone, create a new network:
    1.  [![new ssid](/assets/images/2016/01/new-ssid1-600w.png){: width="300"}](/assets/images/2016/01/new-ssid1.png)
11. In the WLAN Settings tab, click “Show advanced options”:
    1.  [![show advanced options](/assets/images/2016/01/show-advanced-options-600w.png){: width="300"}](/assets/images/2016/01/show-advanced-options.png)
12. Uncheck the “Disable SSID on uplink failure” box:
    1.  [![disable uplink failure](/assets/images/2016/01/disable-uplink-failure-600w.png){: width="300"}](/assets/images/2016/01/disable-uplink-failure.png)
13. Then go through the rest of the steps programming your WLAN
14. After setting up the new network, click on “RF” in the top-right corner and configure your RF settings (These settings keep the radio from making too many dynamic changes during your survey):
    1.  [![rf config](/assets/images/2016/01/rf-config.png){: width="272"}](/assets/images/2016/01/rf-config.png)
15. Once those changes are done, reboot the IAP and you should see your new survey SSID show up after a few minutes

On a side note, I use a 10′ telescoping mast like this for my surveys:

[![2016-01-14 10.40.29](/assets/images/2016/01/2016-01-14-10-40-29-624w.jpg){: .aligncenter width="312"}](/assets/images/2016/01/2016-01-14-10-40-29.jpg)

Questions or comments? Leave them below and/or find me on twitter [@mattbfrederick](https://twitter.com/mattbfrederick)
