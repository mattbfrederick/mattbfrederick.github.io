---
layout: post
title: "Aruba: Temporarily enabling BLE beacon, gathering data, and disabling"
date: 2017-06-06 13:39:56 -0500
permalink: /2017/06/06/aruba-temporarily-enabling-ble-beacon-gathering-data-and-disabling/
description: "Turning on an Aruba AP's USB-powered BLE beacon just long enough to capture its information with show ap debug ble-table, then switching the AP group back to the “no-usb” provisioning profile."
tags: ["aruba", "aruba networks", "beacon", "ble", "wireless"]
---

**We have two AP provisioning profiles, the “default” enables the USB port and the “no-usb” disables the port. AP group is currently referencing the “no-usb”**

```
ap provisioning-profile "default"
!
ap provisioning-profile "no-usb"
usb-power-mode disable
ap-poe-power-optimization enabled

ap-group "Warehouse-Lab"
virtual-ap "TEST"
dot11a-radio-profile "Warehouse-Lab-ARM-a"
dot11g-radio-profile "Warehouse-Lab-ARM-g"
ap-system-profile "BuildingProfile"
provisioning-profile "no-usb"
```

**BLE scanner (android app “BLE Scanner”) next to the AP and picking up no signal**

```
(Test) # show ap database

AP Database
-----------
Name               Group          AP Type  IP Address   Status          Flags  Switch IP    Standby IP
----               -----          -------  ----------   ------          -----  ---------    ----------
40:e3:d6:c5:8a:14  Warehouse-Lab  225      10.42.68.20  Up 23h:42m:20s  2      10.42.68.10  0.0.0.0

Flags: U = Unprovisioned; N = Duplicate name; G = No such group; L = Unlicensed
I = Inactive; D = Dirty or no config; E = Regulatory Domain Mismatch
X = Maintenance Mode; P = PPPoE AP; B = Built-in AP; s = LACP striping
R = Remote AP; R- = Remote AP requires Auth; C = Cellular RAP;
c = CERT-based RAP; 1 = 802.1x authenticated AP; 2 = Using IKE version 2
u = Custom-Cert RAP; S = Standby-mode AP; J = USB cert at AP
i = Indoor; o = Outdoor
M = Mesh node; Y = Mesh Recovery

Total APs:1
```

**One AP up in the database**

```
(Test) #show ap debug ble-table ap-name 40:e3:d6:c5:8a:14
No AP found with ap_name 40:e3:d6:c5:8a:14
```

**Controller returning error on BLE command since it has no BLE turned on (USB port turned off)**

**Configure to AP provisioning profile back to the default (enabling USB port)**

```
(Test) #configure t
Enter Configuration commands, one per line. End with CNTL/Z

(Test) (config) #ap-group Warehouse-Lab
(Test) (AP group "Warehouse-Lab") #provisioning-profile "default"
(Test) (AP group "Warehouse-Lab") #end
(Test) #write mem
Saving Configuration...

Configuration Saved.
```

**After \<1min I see something pop up on my BLE scanner with a strong signal**

**Issue same ap debug command to see BLE table**

```
(Test) #show ap debug ble-table ap-name 40:e3:d6:c5:8a:14

BLE Device Table
----------------
MAC                HW_Type    FW_Ver        Flags   Status  Batt(%)  RSSI  Major#  Minor#  UUID                                  Tx_Power  Last Update  Uptime
---                -------    ------        -----   ------  -------  ----  ------  ------  ----                                  --------  -----------  ------
88:c2:55:ca:cf:3b  LS-BT1USB  OAD B 1.1-25  0x0083  LIA     USB      ---    0       0       4152554E-F99B-4A3B-86D0-947070693A78  14        18s          2m:20s

Total beacons:1

APB UI:[0/NO_UPGRADE_REQD]:65535(0xffff) blks:0/0 rep:0  total:0(0x0)
APB UI:upg_b_status-next:0x00/ooo:0x00/next2:0x00/upg_b:0x00/allrx:0x00/oooBlk:0x00/oooBlk:0x00/oooBlk:0x00
APB UI:upg_b_status_errs-inv_upg:0x00/inv_cmd:0x00/inv_op:0x00/buf_tl:0x00/good:0x00
APB UI:acks/ka-From APB:0x00/0x00 From app:0x00,0x00/0x00
APB UI Clock:Start:1970-01-01 00:00:00  End:1970-01-01 00:00:00  Current:2017-04-04 11:58:52
Note: Battery level for LS-BT1USB devices is indicated as USB.
Note: Uptime is shown as Days hour:minute:second.
Note: Last Update is time in seconds since last heard update.
Status Flags:L:AP's local beacon; I:iBeacon; A: Aruba Beacon; H: Aruba HiPower Beacon
:U:Image Upgrade Pending
```

**We can now capture the beacon mac address and any other needed information**

**Go back and put on the “no-usb” provisioning profile**

```
(Test) #configure t
Enter Configuration commands, one per line. End with CNTL/Z

(Test) (config) #ap-group Warehouse-Lab
(Test) (AP group "Warehouse-Lab") #provisioning-profile "no-usb"
(Test) (AP group "Warehouse-Lab") #end
(Test) #write mem
Saving Configuration...

Configuration Saved.
```

**Verify the BLE beacon is off again by checking BLE scanner and issuing BLE table command**

```
(Test) #show ap debug ble-table ap-name 40:e3:d6:c5:8a:14
No AP found with ap_name 40:e3:d6:c5:8a:14
```
