---
title: Setting up IPSec VPN between Fortigate and Axsguard
author: Maarten Van Driessen
type: post
date: 2016-03-03T06:58:24+00:00
url: /setting-up-ipsec-vpn-between-fortigate-and-axsguard/
categories:
  - Network
tags:
  - Axsguard
  - Fortigate
  - IPSec

---
If you've ever had the "pleasure" of building an IPSec tunnel between 2 endpoints from different vendors, you'll know how smooth that usually goes. Today I was building a tunnel between a Fortigate 70D and an Axsguard Gatekeeper, as you can guess, things didn't go as planned.

## Fortigate

Let's start by creating the tunnel on the Fortigate. Create a new tunnel and select the **Custom VPN Tunnel** template.

{{< figure src="./images/FG-tunnel1.png" alt="FG-tunnel1">}}

 

Next, fill in all the phase 1 settings. In this example, I'm using 3DES and SHA1. While 3DES is still considered as secure, I would recommend against using it in production, mainly because of the speed. If you want to use the public IP address as the local ID, leave the field empty. Fortigate will automatically send its public IP as the local ID. Keep this in mind when you're behind a NAT device.

{{< figure src="./images/FG-phase1.png" alt="FG Phase 1">}}

In the phase 2 settings, you can specify what addresses should be configured on the tunnel. You can choose between a subnet, IP range or a single IP address.

{{< figure src="./images/FG-phase2.png" alt="FG phase 2">}}

You will still need to configure a route and firewall rules for VPN traffic.

## Axsguard

The same principles apply to the configuration of the Axsguard.  One thing to pay attention to is the key lifetime, Fortinet uses seconds to express the lifetime on its devices. Axsguard, on the other hand, uses minutes for the lifetime.

Let's go ahead and create a new tunnel on the Axsguard. Fill in the local parameters. If you can't find the settings that correspond to the settings on the Fortigate, you can add them in the IPSec -IKE, and IPSec - ESP menus. Make sure to set the settings exactly the same as on the Fortigate.

{{< figure src="./images/axs-local-param.png" alt="AXS guard local pparams">}}

Same goes for the remote parameters.

{{< figure src="./images/axs-remote-param.png" alt="AXSguard remote params">}}

The Axsguard will automatically create the required firewall rules to allow traffic to pass through the tunnel.