---
layout: post
title: "Using netsh WLAN show interfaces to Monitor Association/Roaming"
date: 2016-09-17 19:07:22 -0500
permalink: /2016/09/17/using-netsh-wlan-show-interfaces-to-monitor-associationroaming/
description: "In April, John Cosgrove (@rtr_man) posted the following way to monitor your WLAN association statistics that I have been using and playing with since: Once you paste from :loop to goto loop into notepad and save as a .bat file, you will see this little icon: Double-click that and it will open up your command…"
tags: ["command line", "netsh", "wifi", "windows 10", "windows 7", "windows 8", "wlan"]
---

In April, John Cosgrove (@rtr_man) posted the following way to monitor your WLAN association statistics that I have been using and playing with since:

[![cfdpr2\_ueaag4v4](/assets/images/2016/09/cfdpr2_ueaag4v4.jpg)](/assets/images/2016/09/cfdpr2_ueaag4v4.jpg)

Once you paste from :loop to goto loop into notepad and save as a .bat file, you will see this little icon:

[![new1](/assets/images/2016/09/new1.png)](/assets/images/2016/09/new1.png)

Double-click that and it will open up your command line and it will look like this:

[![22](/assets/images/2016/09/22-600w.png)](/assets/images/2016/09/22.png)

If you’ve been working in WiFi for at least a little while, then you should know what most of these mean. Really important ones to pay attention to are BSSID (mac address of the radio of the AP you are connecting to) and Signal (received signal strength from AP).

As you can see, the signal strength is in a percentage. If you want to convert this percentage to dBm, use the following formula (quality=Signal %):

quality = 2 \* (dBm + 100) where dBm: \[-100 to -50\]

dBm = (quality / 2) – 100 where quality: \[0 to 100\]

So a 50% would be a -75 dBm. A 70% would be -65 dBm. 100% is -50 dBm or better. Those are the markers I keep in my head when looking at it.

This is great for real time monitoring of how your client is behaving, but what if you want to record the output? Maybe you want to save the output in a text file? Well, my good WiFi buddy, you are in luck. You would then edit the batch file to look like this:

```bat
:loop
netsh WLAN show interfaces >>WLANstat.txt
timeout /t 5
goto loop
```

You can replace “WLANstat” with whatever you want to name the file.

Now when you run it, the output in the command line will look like this:

[![33](/assets/images/2016/09/33-600w.png)](/assets/images/2016/09/33.png)

Now you’re probably saying “What the heck, Matt?!? Now it’s not showing me any of the statistics! What a dumb blog.”

Take a chill pill, friend, and look at your desktop. Notice anything different? Like maybe a text file that wasn’t there before? You should see a text file appear after you ran the batch file:

[![34](/assets/images/2016/09/34.png)](/assets/images/2016/09/34.png)

Open the text file and now there is the output:

[![35](/assets/images/2016/09/35-464w.png)](/assets/images/2016/09/35.png)

Starting to trust me yet?

OK, so now we have our output being saved in a text file to review later. We’re done here, right? Wrong, good sir, wrong. Wouldn’t it be nice to have some sort of timeline to reference? It would, right? That’s what I thought as well. So, after an hour or so of furious googling, playing, scripting, cursing, googling more, scripting more, cursing more, and having a beer I found a way to get the timestamp in there. Now I am posting it here to hopefully help my WiFi brethren not have to endure what I did.

Edit your batch file to now look like this (by the way, Notepad++ is great for this because you can just right click the file then click “Edit in Notepad++”):

```bat
:loop
set time_hh=%time:~0,2%
if %time_hh% lss 10 (set time_hh=0%time:~1,1%)
set time_mn=%time:~3,2%
set time_ss=%time:~6,2%
set time_ms=%time:~9,2%

echo %time_hh%:%time_mn%:%time_ss%.%time_ms% >> WLANstat_%date:~-4,4%%date:~-7,2%%date:~-10,2%.txt
netsh WLAN show interfaces >>WLANstat_%date:~-4,4%%date:~-7,2%%date:~-10,2%.txt
timeout /t 5
goto loop
```

The string after the filename will append the date in the filename…..just for extra awesomeness….

Now when you run the batch file, the output in the command line should look like this:  
[![36](/assets/images/2016/09/36-600w.png)](/assets/images/2016/09/36.png)

Now you should see a text file appear on your desktop with the date in the filename (YearDayMonth – you can play with the string if you want this different):

[![37](/assets/images/2016/09/37.png)](/assets/images/2016/09/37.png)

When you open the text file, you should see the time above each output (Hour:Minute:Second.Millisecond – you can delete the “set time_ms” line if you don’t want the milliseconds):

[![38](/assets/images/2016/09/38-460w.png)](/assets/images/2016/09/38.png)

So, if you want to document the roaming behavior and perceived signal strength of a Windows client on the network without having to login to the network and look through logs, just open the batch file and walk around and close it when you are done. It’s super quick and easy and gives you some pretty valuable information.

Comment below or tweet me [@mattbfrederick](https://twitter.com/mattbfrederick) with any feedback/questions.

Thanks for reading!

Edit: I found John’s original blog post on this:

[http://wifijohnny.blogspot.com/2016/04/windows-wlan-troubleshooting-with.html](http://wifijohnny.blogspot.com/2016/04/windows-wlan-troubleshooting-with.html)
