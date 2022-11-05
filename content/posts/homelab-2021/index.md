---
title: Homelab 2021
author: Maarten Van Driessen
type: post
date: 2021-04-09T12:09:54+00:00
url: /homelab-2021/
categories:
  - Homelab
tags:
  - design
  - homelab
  - VMware

---
Homelabs...It's a topic that gets a lot of interest everywhere. We're all geeks at heart who like to tinker with hardware and play around and break things. I haven't had a homelab since I sold my Supermicro server back in 2016. Back then, I had access to a lab at my employer and felt that I could no longer justify the cost of running that thing in my basement. Fast forward to 2020 and I find myself really missing that lab.

## Why a homelab

Earlier this year, I decided I would start to learn some more about NSX. My entire VMworld schedule was built around getting as much NSX content as I could. With the VMworld VCP Exam voucher, I tried to get my VCP-NV but ultimately failed.

I ended up scoring 275, which caused a great deal of frustration because I was so close. With the questions that I got, I did feel that it would have been a pass if I had some hands-on experience with the product. So, I decided it was time to take the plunge and invest in a new homelab.

Whenever I talk to other people, in or outside of the industry, about homelabs, the cost is always a big issue that comes up. "Why would you run something in your basement that sits there eating power", "You're going to spend how much on some computers?", ... These are statements we've all probably heard before. But you need to look at it as an investment in yourself, remember that you have to always keep learning. If you stop learning, you'll eventually miss out on opportunities that could mean the next step in your career.

## Design

Before just going out and buying random gear, I figured this exercise is no different than making a new design for a customer. Eventually, I would also like to give VCDX a shot, so I need the practice. So I decided that I would approach this like I would a normal customer interaction.

## Requirements

The first step in the design process is to ask myself what the requirements were. As this is a homelab, some requirements here are not what you would see during a typical engagement. The lab will be running in my basement so noise and power consumption are things that become important. I didn't really want jet engines running in my basement.

I also wanted something that I could expand later on when other use cases or requirements present themselves. This is the list of requirements that I came up with;

  * Support nested virtualization
  * System must be silent
  * System must be relatively low power
  * System should support 10 GbE for future-proofing
  * System should support NVMe drives
  * Solution should be expandable
  * Solution should be able to support GPUs in the future

## Constraints

As with any design, there were some constraints that limited my choices;

  * Budget isn't unlimited
  * Limited 1 GbE ports available on existing switch
  * Limited storage capacity on existing Synology
  * No 10 GbE capabilities present at the moment
  * Since this lab is being housed in my basement, WAF is definitely a thing

## Assumptions

As I could verify a lot of things, I only had 1 real assumption left in my design;

  * Existing NAS capacity is sufficient for backups

This is both an assumption and a risk, the risk being that the capacity is not enough. In a normal customer engagement, I would try to mitigate this risk. But for this scenario, I'm willing to accept the risk, the mitigation here is that I need to purchase additional hardware to provide more capacity.

The design I wrote has a lot more information and decisions in there, but I'm going to keep those for some more blog posts about AMPRS 🙂

## So what did I end up with?

In the end, I decided to build a 4-node all-flash vSAN cluster. Since I'm pretty familiar with Intel CPUs and power consumption was also a thing, I went with an AMD EPYC (Rome) CPU. This also helped with the future proof requirement as this platform supports PCI-e Gen 4 already.

Each host has 8 cores, 128 GB of RAM and 2 NVMe SSDs (500 GB and 2 TB). This should be plenty to test some DCV stuff and to take my first steps with NSX.

## Bill of Materials

This is the part most of you have probably been waiting for, the full BoM can be found below. All prices are in euro and links point to the dutch website [tweakers.net.](tweakers.net)

|Quantity|Item|Price/Item | Total Price|
|--------|----|-----------|------------|
|4|[AMD Epyc 7252 Boxed](https://tweakers.net/pricewatch/1469342/amd-epyc-7252-boxed.html)|€ 375,66|€ 1502,64|
|4|[Asrock Rack ROMED8-2T](https://tweakers.net/pricewatch/1519012/asrock-rack-romed8-2t.html)|€ 574,45|€ 2.297,80|
|4|[Fractal Design Define R5 Black](https://tweakers.net/pricewatch/425679/fractal-design-define-r5-zwart.html)|€ 106|€ 424|
|4|[Samsung FIT Plus 64 GB](https://tweakers.net/pricewatch/1230801/samsung-fit-plus-64gb-zwart.html)|€ 16,95|€ 67,80|
|2|[Netgear Prosafe XS708T](https://tweakers.net/pricewatch/551161/netgear-prosafe-xs708t.html)|€ 519|€ 1.038|
|4|[Noctua NH-U9 TR4-SP3](https://tweakers.net/pricewatch/904091/noctua-nh-u9-tr4-sp3.html)|€ 67,91|€ 271,64|
|12|[Noctua NF-A14 FLX 140 mm](https://tweakers.net/pricewatch/322062/noctua-nf-a14-flx-140mm.html)|€ 20,18|€ 242,16|
|16|[Micron MTA36ASF4G72PZ-3G2J3](https://tweakers.net/pricewatch/1597676/micron-mta36asf4g72pz-3g2j3.html)|€ 188,38|€ 3.014,08|
|4|[Seasonic Focus-PX750](https://tweakers.net/pricewatch/1432188/seasonic-focus-px-750.html)|€ 161,14|€ 644,56|
|4|[Gigabyte Aorus Gen 4 2 TB](https://tweakers.net/pricewatch/1610076/gigabyte-aorus-gen4-ssd-2tb.html)|€ 309|€ 1.236|
|4|[Samsung 980 Pro 500 GB](https://tweakers.net/pricewatch/1603084/samsung-980-pro-500gb.html)|€ 117,90|€ 471,60|
||**Total**||€ 11.210|


I hope this blog can help you in your search for the perfect homelab for your needs. Be sure to check out William Lam's collection of homelabs on [Github](http://vmwa.re/homelab) for more ideas and resources.