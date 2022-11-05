---
title: How to upgrade an IRF stack – incompatible firmware
author: Maarten Van Driessen
type: post
date: 2016-05-24T07:21:32+00:00
url: /upgrade-irf-stack/
categories:
  - Network
tags:
  - Comware
  - IRF
  - ISSU

---
When upgrading HP Comware switches that are part of an IRF stack, you want to use In Service Software Upgrade (ISSU). This allows you to do a rolling upgrade of the firmware while limiting the impact to just 1 switch in the stack.  
ISSU will reboot 1 switch in the stack, wait for it to come back up and then moves on to the next switch.

# Types of ISSU

There are 3 types of ISSU:

  * **Compatible: **This means 2 different software versions can co-exist in the same IRF stack. This is usually for smaller releases, in the same major release
  * **Incompatible: **Typically you will see this when moving between major releases. Only 1 version can exist in the IRF stack
  * **Unknown**

# Upgrading incompatible firmware

## Checking configuration

Before starting the upgrade, you have to check your configuration. Doing an incompatible upgrade means you will create a "split-brain" situation since only 1 version can exist in the same IRF stack.

To prevent this, you have to make sure MAD is configured. You can check this by running the command

<pre class="toolbar:2 show-plain-default:true lang:batch decode:true">display mad verbose</pre>

This will give you an output like this, in this example MAD BFD is used.

<pre class="toolbar:2 show-plain-default:true lang:batch decode:true"> <switch <disp mad verb
Current MAD status: Detect
Excluded ports(configurable):
Excluded ports(can not be configured):
  Ten-GigabitEthernet1/1/1
  Ten-GigabitEthernet1/1/2
  Ten-GigabitEthernet2/1/1
  Ten-GigabitEthernet2/1/2
MAD ARP disabled.
MAD LACP disabled.
MAD BFD enabled interface:
  Vlan-interface200
    mad ip address 4.3.2.1 255.255.255.0 member 1
    mad ip address 4.3.2.2 255.255.255.0 member 2
</pre>

Also, make sure your IRF stack is configured correctly and as desired by running the command

<pre class="toolbar:2 show-plain-default:true lang:batch decode:true">display irf</pre>

## Uploading the image

After you verified that everything is configured correctly, it's time to upload the image. You can do this using any number of methods. Once the image is uploaded to the master node, you need to copy it to all slave nodes. Increment the slot number if you have more than 2 switches in the stack.

copy new5500.bin slot2#flash:

Once the image is uploaded, you can check the compatibility

<pre class="toolbar:2 show-plain-default:true lang:batch decode:true">display version comp-matrix file new5500.bin</pre>

## Starting the upgrade

It's time to actually start upgrading. When the command is issued, the slave node will be loaded with the new firmware. The node will then reboot but will not join the IRF stack (remember, only one version can exist), this will create a split-brain situation that will be resolved by MAD. MAD leaves the IRF ports active but will shut down <span style="text-decoration: underline;">all</span> outgoing interfaces. You might wonder why the IRF interfaces aren't shut down, this is done to allow you to perform the switchover and continue the upgrade.

<pre class="show-plain-default:true lang:batch decode:true">issu load file new5500.bin slot 2 force</pre>

Once the slave node has rebooted, you can perform the switchover. This will reboot the current master node and perform the upgrade to the new firmware version. The command will also enable all external interfaces on the node that's already been upgraded, the new master.

<pre class="toolbar:2 show-plain-default:true lang:batch decode:true">issu run switchover slot 2</pre>

After the node has rebooted, the IRF stack will be fully operational.

If you want the IRF master to be the same as before the upgrade, you can reboot the current master.

<pre class="toolbar:2 show-plain-default:true lang:ps decode:true ">reboot slot 2</pre>

 