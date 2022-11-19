---
title: VCAP6.5-DCV Design Exam Experience
author: Maarten Van Driessen
type: post
date: 2019-05-22T09:00:05+00:00
url: /vcap6-5-dcv-design-exam-experience/
categories:
  - Career
  - Certification
  - VMware
tags:
  - certification
  - design
  - vcap
  - VMware

---
This year, I got the opportunity to go to VMware Empower 2019 in Lisbon. The ticket includes a free exam voucher so I used it to take my VCAP6.5-DCV Design exam. I'm glad to say that I passed! The exam was a lot harder than I expected though.

Having just done the VCP exams, I did not expect the VCAP to be that much harder. In this post, I will try to share some tips that I found useful during preparation and how I experienced the exam

## VCAP

Let's start by clarifying what a VCAP actually is. VCAP stands for VMware Certified Advanced Professional. VMware qualifies the "minimally qualified candidate" as follows in the exam blueprint


> A minimally qualified candidate (MQC) achieving the VMware Certified Advanced Professional 6.5 in Data Center Virtualization Design is capable of developing a conceptual design given a set of customer requirements, determining the functional requirements needed to create a logical design, and architecting a physical design using these elements.

This quote, together with the exam title, give a clear picture of where the focus for this exam is. You are also expected to know all the topics that were handled in the VCP exam and in much greater detail. After all, this is an advanced certification exam.

## Preparation

During my preparation, I read a lot of whitepapers and articles. Below you can find a list of the ones I used. Don't limit yourself to just this list, there's a ton of content out there created by fellow bloggers and VMware itself. Also, make sure to read other posts like this. Most people also link to content that helped them during their preparation.

### Official resources

  * The first thing to do for any is reading the [blueprint][1]. I found the blueprint not that detailed compared to other blueprints. So I looked at the [blueprint][2] for the VCAP6 exam as well
  * [What's new in vSphere 6.5][3]
  * [vSphere Availability][4]
  * [vSphere 6 Fault Tolerance][5] - This whitepaper is based on vSphere 6 but the core architecture remains the same. Be sure to read up on what changed in 6.5!
  * [VMware Validated Designs][6]
  * [Performance best practices][7]
  * [VM encryption performance][8]
  * [Latency sensitivity performance study][9]
  * [vCenter High Availability][10]
  * [vSphere Upgrade guide][11]
  * [vSphere Installation guide][11]
  * [PSC administration guide][11]

#### Community resources

  * [Graham Barker's VCAP prep guide][12] is a must read for everyone in my opinion. It's written for the VCAP6 exam but the design parts still apply to 6.5
  * [David Stamen's tips][13] helped me a lot during my exam, definitely recommend reading them!
  * the vMusketeers created a [VCAP6-DCV Design quiz][14] that you should try for sure. I found that test to be harder than the exam itself. It really tests your knowledge on the core design concepts.
  * [Daniel Paluszek][15] created an extensive blog post with a lot of material in it. I used it a lot.
  * Jeffrey Kusters' [blog][16] on designs, requirements, etc. really helped me understand these concepts

## Exam experience

For me, this was the hardest exam that I've done so far. Questions were multiple choice style and also drag and drop style. Obviously, I can't go talk about the questions but the things I can tell are that you should really be familiar with these concepts

  * RCAR (Requirements, Constraints, Assumptions, and Risks)
  * AMPRS (Availability, Manageability, Performance, Recoverability, Security)
  * RTO (Recovery Time Objective)
  * RPO (Recovery Point Objective)
  * All vSphere features. design and architecture of all components should be known
  * all vSAN features, how to design it and how the architecture works
  * Replication, SRM, ...
  * Conceptual, logical and physical design

Now that I passed the exam, my focus will shift to preparing my upcoming VMUG session and afterwards the VCAP6.5 deploy exam. Exciting times ahead!

I hope that this post gives you a good idea of just how much preparation is required to pass the exam. If you have questions or additions, hit me up!

 [1]: https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/certification/vmw-vcap65-dcv-design-3v0-624-guide.pdf
 [2]: https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/certification/vmw-vcap6-dcv-design-3v0-622-guide.pdf
 [3]: https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/whitepaper/vsphere/vmw-white-paper-vsphr-whats-new-6-5.pdf
 [4]: https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-availability-guide.pdf
 [5]: https://www.vmware.com/files/pdf/techpaper/VMware-vSphere6-FT-arch-perf.pdf
 [6]: https://www.vmware.com/support/pubs/vmware-validated-design-pubs.html
 [7]: https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/performance/Perf_Best_Practices_vSphere65.pdf
 [8]: https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vm-encryption-vsphere65-perf.pdf
 [9]: https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/latency-sensitive-perf-vsphere55-white-paper.pdf
 [10]: https://kb.vmware.com/s/article/2148003
 [11]: https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-652-upgrade-guide.pdf
 [12]: http://virtualg.uk/vcap-dcv-design-exam-preparation/
 [13]: https://davidstamen.com/2017/12/21/vcap6.5-dcv-design-experience/
 [14]: http://vmusketeers.com/vcap6-dcv-design-quiz/
 [15]: https://www.paluszek.com/wp/2018/06/18/achievement-unlocked-vmware-vcap-6-5-dcv-3v0-624-exam-summary-and-tips/
 [16]: https://www.jeffreykusters.nl/2018/06/25/breaking-down-the-conceptual-design-rcars-and-amprs-vcdx-style/