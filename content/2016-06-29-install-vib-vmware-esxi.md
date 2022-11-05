---
title: Install VIB on VMware ESXi
author: Maarten Van Driessen
type: post
date: 2016-06-29T18:02:18+00:00
url: /install-vib-vmware-esxi/
categories:
  - VMware
tags:
  - homelab
  - VIB
  - VMware

---
Today I got my shiny new host for the homelab. After installing ESXi, I noticed the 10 Gbit NICs weren't being detected. After looking around a bit I found the drivers in the form of a VIB.

Start by uploading the VIB to a datastore that is accessible to the host. Turn on the SSH service in the security section and fire up a session.

Once connected run

<pre class="plain:false plain-toggle:false show-plain-default:true lang:batch decode:true ">esxcli software vib install -v /vmfs/volumes/NAS1-iSCSI/net-ixgbe_4.4.1-1OEM.600.0.0.2159203.vib</pre>

If everything goes well you should get an output like this

<pre class="plain:false plain-toggle:false show-plain-default:true lang:batch decode:true ">Installation Result
   Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
   Reboot Required: true
   VIBs Installed: INT_bootbank_net-ixgbe_4.4.1-1OEM.600.0.0.2159203
   VIBs Removed: VMware_bootbank_net-ixgbe_3.7.13.7.14iov-20vmw.600.0.0.2494585
   VIBs Skipped:
</pre>

Once all is done, reboot the host and you'll see the installed device pop up, in my case the 10 Gbit NICs