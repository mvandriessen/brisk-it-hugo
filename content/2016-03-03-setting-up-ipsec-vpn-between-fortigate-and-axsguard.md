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

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-tunnel1.png" rel="attachment wp-att-51"><img loading="lazy" class="aligncenter wp-image-51 " src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-tunnel1.png?resize=440%2C319" alt="FG-tunnel1" width="440" height="319" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-tunnel1.png?w=536&ssl=1 536w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-tunnel1.png?resize=300%2C218&ssl=1 300w" sizes="(max-width: 440px) 100vw, 440px" data-recalc-dims="1" /></a>

 

Next, fill in all the phase 1 settings. In this example, I'm using 3DES and SHA1. While 3DES is still considered as secure, I would recommend against using it in production, mainly because of the speed. If you want to use the public IP address as the local ID, leave the field empty. Fortigate will automatically send its public IP as the local ID. Keep this in mind when you're behind a NAT device.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase1.png" rel="attachment wp-att-54"><img loading="lazy" class="aligncenter wp-image-54 " src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase1.png?resize=497%2C563" alt="FG-phase1" width="497" height="563" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase1.png?w=660&ssl=1 660w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase1.png?resize=265%2C300&ssl=1 265w" sizes="(max-width: 497px) 100vw, 497px" data-recalc-dims="1" /></a>

In the phase 2 settings, you can specify what addresses should be configured on the tunnel. You can choose between a subnet, IP range or a single IP address.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase2.png" rel="attachment wp-att-55"><img loading="lazy" class="aligncenter wp-image-55 " src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase2.png?resize=483%2C480" alt="FG-phase2" width="483" height="480" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase2.png?w=656&ssl=1 656w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase2.png?resize=150%2C150&ssl=1 150w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/FG-phase2.png?resize=300%2C298&ssl=1 300w" sizes="(max-width: 483px) 100vw, 483px" data-recalc-dims="1" /></a>

 

You will still need to configure a route and firewall rules for VPN traffic.

## Axsguard

The same principles apply to the configuration of the Axsguard.  One thing to pay attention to is the key lifetime, Fortinet uses seconds to express the lifetime on its devices. Axsguard, on the other hand, uses minutes for the lifetime.

Let's go ahead and create a new tunnel on the Axsguard. Fill in the local parameters. If you can't find the settings that correspond to the settings on the Fortigate, you can add them in the IPSec -IKE, and IPSec - ESP menus. Make sure to set the settings exactly the same as on the Fortigate.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-local-param.png" rel="attachment wp-att-52"><img loading="lazy" class="aligncenter wp-image-52" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-local-param.png?resize=598%2C643" alt="Axsguard local parameters" width="598" height="643" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-local-param.png?w=776&ssl=1 776w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-local-param.png?resize=279%2C300&ssl=1 279w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-local-param.png?resize=768%2C825&ssl=1 768w" sizes="(max-width: 598px) 100vw, 598px" data-recalc-dims="1" /></a>

 

Same goes for the remote parameters.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-remote-param.png" rel="attachment wp-att-53"><img loading="lazy" class="aligncenter wp-image-53" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-remote-param.png?resize=632%2C710" alt="Axsguard remote parameters" width="632" height="710" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-remote-param.png?w=767&ssl=1 767w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/02/axs-remote-param.png?resize=267%2C300&ssl=1 267w" sizes="(max-width: 632px) 100vw, 632px" data-recalc-dims="1" /></a>

 

The Axsguard will automatically create the required firewall rules to allow traffic to pass through the tunnel.