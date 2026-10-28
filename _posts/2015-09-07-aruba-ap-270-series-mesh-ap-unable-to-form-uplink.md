---
layout: post
title: "Aruba AP-270 series mesh AP unable to form uplink"
date: 2015-09-07 17:23:16 -0500
permalink: /2015/09/07/aruba-ap-270-series-mesh-ap-unable-to-form-uplink/
description: "Problem: AP-270 (had this happen on a 274 and a 275) does not join the controller. I saw this on both a Mesh Portal and Mesh Point. After verifying proper power, consoling into the ap showed the following logs at the end of the boot cycle: ~ # [   40.013529] wl0: set MESH_ID(mesh-secshack, 13) [  …"
tags: ["ap-270", "aruba networks", "mesh", "uplink", "vap", "wifi"]
---

**Problem:**

AP-270 (had this happen on a 274 and a 275) does not join the controller. I saw this on both a Mesh Portal and Mesh Point. After verifying proper power, consoling into the ap showed the following logs at the end of the boot cycle:

```
~ # [   40.013529] wl0: set MESH_ID(mesh-secshack, 13)
[   40.468951] asap_vap_gone: Got vap gone notification on unknown vap:0:1
[   41.620979] asap_vap_gone: Got vap gone notification on unknown vap:0:0
```

Now, you can see that it acquired the correct mesh group, but those other two logs are concerning. A virtual ap group is something you kind of want your ap to have.

**Research:**

The log “asap_vap_gone: Got vap gone notification on unknown vap” means that the ap cannot form an uplink. It is normally associated with a misconfiguration with LACP (which is most of what you will find online). But, if you are working in a non-LACP environment, then it doesn’t help you out too much.

Here is Aruba TAC’s definition: “This message indicates that the AP is not able to form a bonding with the uplink. This is commonly seen when the AP unable to form a bond with an uplink switch (with LACP configured) due to issues in LACP configuration or when the mesh point unable to form a connection with the mesh portal, (usually due to channel mismatch).”

**Fix:**

To fix the issue, you copy the configuration of a working AP to the non-working one (MAKE SURE YOU CHANGE THE NAME!!). To do this, you stop the ap boot cycle by pressing Enter continuously after powering on the AP (don’t try to wait until it prompts you, you won’t hit it in time – trust me). The, from the ap boot menu, follow the instructions below.

To apply the running configuration from the working AP to nonworking AP.

*Sample configuration*

```
Apboot>printenv

bootdelay=2
baudrate=9600
autoload=n
boardname=Palomino
bootcmd=boot=ap
autostart=yes
bootfile=mips64.ari
ethaddr=00:24:6c:c3:cb:56
num_ipsec_retry=85
name=00:24:6c:c3:cb:56
group=MESH-GRP
ip6prefix=64
servername=aruba-master
a_ant_gain=ee3efa2492d0e7145c6414f50ffe9d8396540310000x002
g_ant_gain=ee3efa2492d0e7145c6414f50ffe9d8396540310000x002
ap70_ext_ant=1
a_antenna=0
g_antenna=0
usb_type=0
uplink_vlan=0
auto_prov_id=0
is_rmp_enable=0
priority_ethernet=0
priority_cellular=0
cellular_nw_preference=1
usb_power_mode=0
cert_cap=0
mesh_role=1
installation=0
mesh_sae=0
start_type=cold_start
numcores=1
stdin=serial
stdout=serial
stderr=serial
ethact=en0
```

*to apply this configuration to the nonworking AP, access the boot mode of the AP and ( add “setenv” before the command and remove “=” from the printenv output)*

```
setenv bootdelay 2
setenv baudrate 9600
setenv autoload n
setenv boardname Palomino
setenv bootcmd boot ap
setenv autostart yes
setenv bootfile mips64.ari
setenv ethaddr 00:24:6c:c3:cb:56
setenv num_ipsec_retry 85
setenv name 00:24:6c:c3:cb:56
setenv group MESH-GRP
setenv ip6prefix 64
setenv servername aruba-master
setenv a_ant_gain ee3efa2492d0e7145c6414f50ffe9d8396540310000x002
setenv g_ant_gain ee3efa2492d0e7145c6414f50ffe9d8396540310000x002
setenv ap70_ext_ant 1
setenv a_antenna 0
setenv g_antenna 0
setenv usb_type 0
setenv uplink_vlan 0
setenv auto_prov_id 0
setenv is_rmp_enable 0
setenv priority_ethernet 0
setenv priority_cellular 0
setenv cellular_nw_preference 1
setenv usb_power_mode 0
setenv cert_cap 0
setenv mesh_role 1
setenv installation 0
setenv mesh_sae 0
setenv start_type cold_start
setenv numcores 1
setenv stdin serial
setenv stdout serial
setenv stderr serial
setenv ethact en0
```

after running the command save the configuration by running the command

```
saveenv
```

after saving the configuration run the command “boot” to reboot the AP.

**Notes:**

So, the above worked. Also, changing the AP completely worked. Unfortunately, this was in a production environment and I wasn’t in a spot where I could dig too much on it. So I was unable to find out what exactly was causing my issue. Wanted to publish this to show a possible workaround and because literature about this online is very limited.

Questions or comments? Leave them below and/or find me on twitter [@mattbfrederick](https://twitter.com/mattbfrederick)
