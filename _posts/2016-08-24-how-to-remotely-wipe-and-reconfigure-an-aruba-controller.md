---
layout: post
title: "Aruba: How to Remotely Wipe and Reconfigure an Aruba Controller"
date: 2016-08-24 14:25:01 -0500
permalink: /2016/08/24/how-to-remotely-wipe-and-reconfigure-an-aruba-controller/
description: "I’ve recently been on a project that required me to be able to remotely wipe and reconfigure an Aruba controller without having console access. I have yet to run into this before and had to do some playing around to make it work. I tried making a config file in Notepad and saving it as…"
tags: ["aruba", "controller", "mobility", "remote", "wifi", "wipe"]
---

I’ve recently been on a project that required me to be able to remotely wipe and reconfigure an Aruba controller without having console access. I have yet to run into this before and had to do some playing around to make it work. I tried making a config file in Notepad and saving it as a .cfg and having the controller boot to that, but the passwords never worked and I would have to have someone break into it using a console. Then, when trying to configure other settings that referenced default configuration lines – they wouldn’t work. The only sure way I could get it to work every time is by running the configuration through a controller first and then saving the .cfg file off the controller and using that. If anyone has anything better, leave a comment or hit me up on Twitter [@mattbfrederick](https://twitter.com/mattbfrederick)

1\. Create a script that can be copied and pasted into a controller

```
hostname ZaphodBeeblebrox
!
enable bypass
!
banner motd ~
"NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE"
"This is a private network device. If you are not authorized to"
"access this device, disconnect at once! Actual or attempted use,"
"access, examination, or configuration change by an unauthorized"
"person external to the Corporation will result in criminal and civil"
"prosecution to the fullest extent of the law. Unauthorized login"
"attempts by internal personnel will be reported immediately to the"
"violator's management and may result in disciplinary action."
"NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE"
~
!
clock timezone UTC 0
!
license profile centralized-licensing-enable
!
ipv6 enable
!
ipv6 mld
!
vlan 2 Native
vlan 42 MGMT
!

interface vlan 850
ip address 10.10.10.42 255.255.255.0
!
Interface Loopback
no ip address
!
ip default-gateway 10.10.10.1
!
interface gigabitethernet 0/0/2
description "Link to SomeSwitch"
trusted
ip access-group "mcast_opt" session
switchport mode trunk
switchport trunk native vlan 2
switchport trunk allowed vlan 42
!
interface gigabitethernet 0/0/0
description "Undefined"
trusted
ip access-group "mcast_opt" session
switchport mode trunk
switchport trunk native vlan 2
switchport trunk allowed vlan 42
!
interface vlan 1
no ip address
shutdown
!
syslocation "Magrathea"
syscontact "DentArthurDent"
snmp-server community "KnowWhereYoureTowelIs"
!
mgmt-server type amp primary-server 10.10.10.43 profile default-amp
!
aaa authentication-server tacacs "AAAserver"
host "10.10.10.50"
key PanGalacticGoggleBlaster
session-authorization
!
aaa authentication mgmt
server-group "mgmt"
enable
!
aaa tacacs-accounting server-group mgmt mode enable command all
!
snmp-server enable trap
snmp-server host 10.10.10.100 version 2c KnowWhereYoureTowelIs udp-port 162
snmp-server host 10.10.10.101 version 2c KnowWhereYoureTowelIs udp-port 162
snmp-server trap source 10.10.10.42
logging level warnings security
logging level warnings network
logging level warnings system
logging level warnings user
logging level warnings wireless
!
localip 0.0.0.0 ipsec ThrowSelfAtGround&Miss
!
enable secret
password
password
!

write memory

mgmt-user admin root
password
password
!
```

2\. Grab any controller that is running the same AOS version and has the same kind of licenses (AP, PEF, RFP, etc) and console into it  
3\. Wipe the controller by doing a *write erase* (Note: do not do a *write erase all* because it will wipe out your licenses)

```
(Trillian) #write erase
All the configuration will be deleted. Press 'y' to proceed :
Write Erase successful

(Trillian) #reload
Do you really want to restart the system(y/n): y
System will now restart!
```

4\. Go through the setup wizard a day just hit enter on everything (besides putting on an ipv6 address as that actually requires some input) and give generic administration and enable password and reboot

```
Enter Switch Role (master|local|standalone) [master]:
Enter VLAN 1 interface IP address [172.16.0.254]:
Enter VLAN 1 interface subnet mask [255.255.255.0]:
Enter IP Default gateway [none]:
Do you wish to configure IPV6 address on vlan 1 (yes|no) [yes]: no
This controller is restricted to Country code US for United States, please confirm (yes|no)?: yes
Enter Time Zone [PST-8:0]:
Enter Time in UTC [02:49:39]:
Enter Date (MM/DD/YYYY) [8/12/2016]:
Enter Password for admin login (up to 32 chars): ********
Re-type Password for admin login: ********
Enter Password for enable mode (up to 15 chars): ********
Re-type Password for enable mode: ********
Do you wish to shutdown all the ports (yes|no)? [no]: no

Current choices are:

System name: Aruba7005
Switch Role: master
VLAN 1 interface IP address: 172.16.0.254
VLAN 1 interface subnet mask: 255.255.255.0
Option to configure VLAN 1 interface IPV6 address: no
Time Zone: PST-8:0
Ports shutdown: no

If you accept the changes the switch will restart!
Type  to go back and change answer for any question
Do you wish to accept the changes (yes|no)yes
```

5\. Login to your now “clean” controller, do a *config terminal* and paste in your script

```
(Aruba7005)
User: admin
Password: ********
(Aruba7005) >en
Password:********
(Aruba7005) #configure t
Enter Configuration commands, one per line. End with CNTL/Z

(Aruba7005) (config) #hostname ZaphodBeeblebrox
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #enable bypass
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #banner motd ~
Enter TEXT message [maximum of 4095 characters].
Each line in the banner message should not exceed 255 characters.
End with the character '~'.

"NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE"
"This is a private network device. If you are not authorized to"
"access this device, disconnect at once! Actual or attempted use,"
"access, examination, or configuration change by an unauthorized"
"person external to the Corporation will result in criminal and civil"
"prosecution to the fullest extent of the law. Unauthorized login"
"attempts by internal personnel will be reported immediately to the"
"violator's management and may result in disciplinary action."
"NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE NOTICE"
~
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #clock timezone UTC 0
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #license profile centralized-licensing-enable
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #ipv6 enable
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #ipv6 mld
(ZaphodBeeblebrox) (config-mld) #!
(ZaphodBeeblebrox) (config) #vlan 2 Native
(ZaphodBeeblebrox) (config) #vlan 42 MGMT
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #
(ZaphodBeeblebrox) (config) #interface vlan 42
(ZaphodBeeblebrox) (config) #   ip address 10.10.10.42 255.255.255.0
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #Interface Loopback
(ZaphodBeeblebrox) (config-loop)#  no ip address
(ZaphodBeeblebrox) (config-loop)#!
(ZaphodBeeblebrox) (config) #ip default-gateway 10.10.10.1
Warning: Route will be added when nexthop is resolved
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #interface gigabitethernet 0/0/2
(ZaphodBeeblebrox) (config-if)# description "Link to SomeSwitch"
(ZaphodBeeblebrox) (config-if)# trusted
(ZaphodBeeblebrox) (config-if)# ip access-group "mcast_opt" session
(ZaphodBeeblebrox) (config-if)# switchport mode trunk
(ZaphodBeeblebrox) (config-if)# switchport trunk native vlan 2
(ZaphodBeeblebrox) (config-if)# switchport trunk allowed vlan 42
(ZaphodBeeblebrox) (config-if)#!
(ZaphodBeeblebrox) (config) #interface gigabitethernet 0/0/0
(ZaphodBeeblebrox) (config-if)# description "Undefined"
(ZaphodBeeblebrox) (config-if)# trusted
(ZaphodBeeblebrox) (config-if)# ip access-group "mcast_opt" session
(ZaphodBeeblebrox) (config-if)# switchport mode trunk
(ZaphodBeeblebrox) (config-if)# switchport trunk native vlan 2
(ZaphodBeeblebrox) (config-if)# switchport trunk allowed vlan 42
(ZaphodBeeblebrox) (config-if)#!
(ZaphodBeeblebrox) (config) #interface vlan 1
(ZaphodBeeblebrox) (config-subif)#no ip address
(ZaphodBeeblebrox) (config-subif)#shutdown
(ZaphodBeeblebrox) (config-subif)#!
(ZaphodBeeblebrox) (config) #syslocation "Magrathea"
(ZaphodBeeblebrox) (config) #syscontact "DentArthurDent"
(ZaphodBeeblebrox) (config) #snmp-server community "KnowWhereYoureTowelIs"
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #mgmt-server type amp primary-server 10.10.10.43 profile default-amp
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #aaa authentication-server tacacs "AAAserver"
(ZaphodBeeblebrox) (TACACS Server "AAAserver") #   host "10.10.10.50"
(ZaphodBeeblebrox) (TACACS Server "AAAserver") #   key PanGalacticGoggleBlaster
(ZaphodBeeblebrox) (TACACS Server "AAAserver") #   session-authorization
(ZaphodBeeblebrox) (TACACS Server "AAAserver") #!
(ZaphodBeeblebrox) (config) #aaa authentication mgmt
(ZaphodBeeblebrox) (Management Authentication Profile) #   server-group "mgmt"
(ZaphodBeeblebrox) (Management Authentication Profile) #   enable
(ZaphodBeeblebrox) (Management Authentication Profile) #!
(ZaphodBeeblebrox) (config) #aaa tacacs-accounting server-group mgmt mode enable command all
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #snmp-server enable trap
(ZaphodBeeblebrox) (config) #snmp-server host 10.10.10.100 version 2c KnowWhereYoureTowelIs udp-port 162
(ZaphodBeeblebrox) (config) #snmp-server host 10.10.10.101 version 2c KnowWhereYoureTowelIs udp-port 162
(ZaphodBeeblebrox) (config) #snmp-server trap source 10.10.10.42
(ZaphodBeeblebrox) (config) #logging level warnings security
(ZaphodBeeblebrox) (config) #logging level warnings network
(ZaphodBeeblebrox) (config) #logging level warnings system
(ZaphodBeeblebrox) (config) #logging level warnings user
(ZaphodBeeblebrox) (config) #logging level warnings wireless
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #localip 0.0.0.0 ipsec ThrowSelfAtGround&Miss
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #enable secret
Password:********
Re-Type password:********
(ZaphodBeeblebrox) (config) #!
(ZaphodBeeblebrox) (config) #
(ZaphodBeeblebrox) (config) #write memory
Saving Configuration...

Configuration Saved.

(ZaphodBeeblebrox) (config) #
(ZaphodBeeblebrox) (config) #mgmt-user admin root
Password:********
Re-Type password:********
Password changed. Please re-authenticate
```

6\. Check the boot file of the controller by doing a *show boot* (it should be default.cfg) and do a *write memory*

```
(ZaphodBeeblebrox) # show boot
Config File: default.cfg

Boot Partition: PARTITION 1

(ZaphodBeeblebrox) #write mem
Saving Configuration...

Configuration Saved.
```

7\. Insert a USB into the controller. Verify you can see its contents by doing a *dir usb: partition \[0\|1\]*

```
(ZaphodBeeblebrox) #dir usb: partition 1
Listing /mnt/usbdisk/slot0/1

drwxr-xr-x    3 root     root        16384 Apr 28  2014 $RECYCLE.BIN
drwxr-xr-x    2 root     root        16384 Jul  9  2014 ODB$
-rwxr-xr-x    1 root     root    3839401984 Jul 29 21:22 SW_DVD5_WIN_ENT_10_1511_64BIT_English_MLF_X20-82288.ISO

(ZaphodBeeblebrox) #
```

8\. Copy the configuration from the controller to the USB drive by doing a copy flash: *default.cfg usb: partition 1 .cfg* (adjust the filename from flash or USB partition if yours is different)

```
(ZaphodBeeblebrox) #copy flash: default.cfg usb: partition 1 Cutoverconfig.cfg
....File default.cfg copied to /mnt/usbdisk/slot0/1/Cutoverconfig.cfg successfully.
```

9\. Do an *eject usb*

10\. Take your new configuration file and copy it to the controller you wish to wipe using *copy : flash:*

```
(FordPrefect) #copy usb: partition 1 Cutoverconfig.cfg flash: Cutoverconfig.cfg
....File Cutoverconfig.cfg copied to flash successfully.
```

11\. Verify it was successfully copied by doing a *dir*

```
(FordPrefect) #dir

-rw-r--r--    1 root     root         6331 Aug 12 02:50 AUDITTRAIL-HISTORY.log
-rw-r--r--    1 root     root         3494 Aug 12 02:50 AUDITTRAIL-LOGIN_OUT-HISTORY.log
-rwxr-xr-x    1 root     root        24664 Aug 12 03:04 Cutoverconfig.cfg
```

12\. Now edit the boot file of the controller by doing a *boot config-file*

```
(FordPrefect) #boot config-file Cutoverconfig.cfg
```

13\. Verify that it is now the boot file by doing a *show boot*

```
(FordPrefect) #show boot
Config File: Cutoverconfig.cfg

Boot Partition: PARTITION 1
```

14\. Reload the controller by doing a *reload* (if it asks you to save the configuration, select NO)

```
(FordPrefect) #reload
Do you want to save the configuration(y/n): n
Do you really want to restart the system(y/n):
```

15\. The controller will now boot up using the new configuration file.  
16\. Login to the controller, do an enable then do a *boot config-file default.cfg* and then a *write memory*

```
(FordPrefect) #boot config-file default.cfg
(FordPrefect) #show boot
Config File: default.cfg

Boot Partition: PARTITION 1

(FordPrefect) #write mem
Saving Configuration...

Configuration Saved.
```

Done!
