---
title: Clear DNS cache VCSA/PhotonOS
author: Maarten Van Driessen
type: post
date: 2020-10-21T10:55:54+00:00
url: /clear-dns-cache-vcsa-photonos/
categories:
  - VMware
tags:
  - dns
  - vcenter
  - vcsa
  - VMware

---
The last few weeks I've had to do a couple of IP changes on ESXi hosts. This always goes without a lot of issues but it can be annoying when you have to wait for the new IP address to be updated in DNS and then for it to be visible in vCenter. The quickest way to get the new DNS records, is to clear the DNS cache in VCSA. Since VCSA is based on PhotonOS, this will also work on other PhotonOS VMs.

PhotonOS uses dnsmasq as a local DNS cache/proxy. So all we have to do is restart that service to clear the cache.

First, open up an SSH session to VCSA and enter the bash shell.
{{< figure src="./images/vcsa-logon.png" alt="vcsa logon">}}

Run the following systemctl command to restart the service.

```systemctl restart dnsmasq.service ```

{{< figure src="./images/systemctl-restart-dnsmasq.png" alt="restart dnsmasq">}}

Now, we just have to check that the service is up and running again. We can use systemctl for that as well.

```systemctl status dnsmasq.service```

{{< figure src="./images/dnsmasq-status.png" alt="dnsmasq status">}}

If all is well you should see the output like it's shown above. Run a quick ping to check that the DNS record is resolving to the correct IP and you're done!

This was a quick post but I kept having to google it. Hope this helps !