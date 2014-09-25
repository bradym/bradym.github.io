---
layout      : post
category    : Misc
title       : "Setting up Wireless with DSL"
keywords    : "DSL, wireless, cisco 678"
description : "Using a Cisco 678 DSL modem and want to setup wireless? It's trickier than it sounds, here's how I got it to work."
date        : 2008-01-02
permalink   : /misc/setting-up-wireless-with-dsl.html
---
It turns out that setting up wireless with DSL is a bit different than
with cable modem, here's how I got it to work.

In order to get it working I used a Cisco 678 DSL Modem, Linksys WRT54G
Wireless Router and a Basic 5 port hub.

Here's the process that I went through to get it working.

### Step 1: Configure Cisco 678

The first thing I did was to enable the DHCP Server on the Cisco 678. Be
sure to set the starting IP address and DNS servers on the Cisco, or it
won't work properly.

DSL Modem Setup:

-   IP Address: 192.168.1.1
-   Subnet Mask: 255.255.255.0
-   Starting IP Address: 192.168.1.100

### Step 2: Configure Linksys WRT54G

Here's the part that I had to call tech support for help with. The
wireless router must be on a different subnet than the Cisco, or it will
not work properly.

Wireless Router Setup:

-   IP Address: 192.168.2.1
-   Subnet Mask: 255.255.255.128
-   Starting IP Address: 192.168.2.100

The wireless router should be setup to get an IP address automatically,
which is the default setting on the Linksys.

I also set the DNS servers on the Linksys to make sure that they would
be set properly on client computers. It may work without this, but
setting it here avoids possible problems.

### Step 3: Hook it all together

Run a cable from the Cisco 678 to the uplink port in the hub. Then plug
the wireless router into the hub as well, and you should be good to go.

### Additional Notes:

-   Computers plugged into the hub will be in the 192.168.1 subnet.
-   Computers plugged into the Linksys will be in the 192.168.2 subnet.

If you plan to use your network for gaming or other network
applications, it is important to have all of the computers on the same
subnet. This would mean plugging any computers without wireless cards
into the Linksys.
