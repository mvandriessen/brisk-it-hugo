---
title: Using VLANs on Fortigate 30D
author: Maarten Van Driessen
type: post
date: 2016-02-26T08:36:28+00:00
url: /using-vlans-on-fortigate-30d/
categories:
  - Network
tags:
  - Fortigate
  - VLAN

---
While setting up a new Fortigate 30D for a client, I wanted to add a new VLAN for the guest Wi-Fi network. Usually, you just go into **Network** - **Interfaces** and add a new Interface there.

On the 30D however, this option wasn't there. After changing the device from switch mode to interface mode and back, I figured you can't do it in the GUI. The only way to do it on a 30D is by using the CLI.

To add a new VLAN, go to the dashboard, open up the CLI and enter these commands:
```
  config system interface
  edit GuestWifi
  set type vlan
  set vlanid 100
  set interface vlan
  set ip 172.16.100.1/24
  next
  end
```

After running these commands, you'll also see the VLAN show up in the **Network - Interfaces** page.