---
title: 'Quick post: Bug in Veeam Quick Migration'
author: Maarten Van Driessen
type: post
date: 2017-09-20T06:25:07+00:00
url: /quick-post-bug-in-veeam-quick-migration/
categories:
  - Veeam
tags:
  - Veeam
  - VMware

---
Last week I was moving some VMs for a client using Veeam Quick Migration. During the migration, I happened to stumble upon some strange behavior.

The source VMs make up an Oracle RAC cluster, using shared VMDKs. One of the requirements for setting up shared VMDKs, using the multi-writer option, is that they are thick eager zeroed disks. When the VMs got to the other side, they wouldn't boot. I figured something went wrong during the migration so I tried it again. Once the second run completed, the VMs still wouldn't boot so I started digging around some more.

The error message I was getting was rather vague; _"Incompatible device backing specified for device &#8216;0'."_. After verifying the config of both nodes I eventually decided to look at the disk type on the destination side. That's when I noticed the disk type was thick provisioned lazy zeroed. Ahah, that's why they didn't want to boot! After manually inflating the disks, they were up and running again. I'm starting to suspect that this is a bug.

## Running some tests

I started building some more test VMs just to prove that this was, in fact, a bug. One of the options you can set during the Quick Migration wizard, is the disk type. You can explicitly select each of the types, or you can have Veeam use the same format as on the source side. Explicitly selecting thick provisioned eager zeroed or using the same as source also produced a VM with lazy zeroed disks. Time to submit a ticket!

As usual, Veeam support was very helpful and investigated the issue. A couple days later they came back to me and confirmed this was indeed a bug that will be fixed in an upcoming version.

## Workaround

This bug is a minor inconvenience since there is an easy workaround. You can login to an ESXi server using SSH and convert the VMDK using the command

<pre class="lang:default decode:true">vmkfstools --eagerzero /vmfs/volumes/DatastoreName/VMName/VMName.vmdk</pre>

More info on how to convert a VMDK can be found in this [VMware KB.][1]

 [1]: https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1035823&src=vmw_so_vex