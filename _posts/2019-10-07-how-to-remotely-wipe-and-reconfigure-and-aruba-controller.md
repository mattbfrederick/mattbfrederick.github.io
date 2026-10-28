---
layout: post
title: "How to remotely wipe and reconfigure an Aruba controller"
date: 2019-10-07 15:47:15 -0600
permalink: /2019/10/07/how-to-remotely-wipe-and-reconfigure-and-aruba-controller/
description: "1. Create a script that can be copied and pasted into a controller 2. Grab any controller that is running the same AOS version and has the same kind of licenses  (AP, PEF, RFP, etc) and console into it 3. Wipe the controller by doing a write erase (Note: do not do a write erase…"
---

1\. Create a script that can be copied and pasted into a controller

2\. Grab any controller that is running the same AOS version and has the same kind of licenses (AP, PEF, RFP, etc) and console into it

3\. Wipe the controller by doing a *write erase* (Note: do not do a *write erase all* because it will wipe out your licenses)

4\. Go through the setup wizard a day just hit enter on everything (besides putting on an ipv6 address as that actually requires some input) and give generic administration and enable password and reboot

5\. Login to your now “clean” controller, do a *config terminal* and paste in your script

6\. Check the boot file of the controller by doing a *show boot* (it should be defult.cfg) and do a *write memory*

7\. Insert a USB into the controller. Verify you can see its contents by doing a *dir usb: partition \[0\|1\]*

8\. Copy the configuration from the controller to the USB drive by doing a *copy flash: default.cfg usb: partition 1 \<filename\>.cfg* (adjust the filename from flash or USB partition if yours is different)

9\. Take your new configuration file and tftp it to the controller you wish to wipe using *copy tftp: \<iPhone address\> \<filename\> flash: \<filename\>*

10\. Verify it was successfully copied by doing a *dir*

11\. Now edit the boot file of the controller by doing a *boot config-file \<filename\>*

12\. Verify that it is now the boot file by doing a *show boot*

13\. Reload the controller by doing a *reload* **(if it asks you to save the configuration, select NO)**

14\. The controller will now boot up using the new configuration file.

15\. Login to the controller, do an *enable* then do a *boot config-file default.cfg* and then a *write memory*

Done!
