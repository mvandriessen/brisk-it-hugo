---
title: 'vCenter update fails: vCenter server is non-operational'
author: Maarten Van Driessen
type: post
date: 2020-09-04T13:38:49+00:00
url: /vcenter-update-fails-vcenter-server-is-non-operational/
categories:
  - VMware
tags:
  - vcenter
  - VMware

---
Today, while I was updating vCenter in my lab, I ran into a strange issue. The update I was installing failed, when I wanted to try again, this ominous error message popped up.
{{< figure src="./images/non-operational.png" alt="vcenter non-operational">}}

vCenter being non-operational left me a bit of a doom and gloom feeling but, thankfully, the fix is rather easy.

## Fixing the issue

Open up an SSH session to your vCenter server and run the command below to remove the software\_update\_state.conf file.

```rm /etc/applmgmt/appliance/software_update_state.conf```

You can check the contents of the file by opening it with vi, an example is shown below.

{{< figure src="./images/updatestateconf.png" alt="update state.conf">}}

 

Turns out that the state "INSTALL\_FAILED" is checked by the python script that performs the installation. The script can be found in /usr/lib/applmgmt/update/py/vmware/appliance/update/update\_state.py

{{< figure src="./images/installfailed.png" alt="installfailed">}}

Removing the file will make this check pass and the installation will continue.