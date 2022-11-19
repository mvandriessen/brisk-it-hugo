---
title: 'ERROR: The host returns esxupdate error code:15'
author: Maarten Van Driessen
type: post
date: 2018-10-11T13:00:01+00:00
url: /error-the-host-returns-esxupdate-error-code15/
categories:
  - VMware
tags:
  - Lenovo
  - VIB
  - VMware
  - VUM

---
Today I was preparing some updates with VUM on Lenovo servers running vSphere 6.0. Things did not go as expected. While staging the patches I was greeted with this error.

**ERROR: The host returns esxupdate error code:15. The package manager transactions is not successful. Check the update Manager log files and esxupdate.log files for more details.**

Looking into the esxupdate.log file I could find the following entries:

``` 2018-10-08T08:23:18Z esxupdate: 31744883: BootBankInstaller.pyc: ERROR: The pending transaction requires 242 MB free space, however the maximum supported size is 239 MB. ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR: An esxupdate error exception was caught: ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR: Traceback (most recent call last): ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/usr/sbin/esxupdate", line 238, in main ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:     cmd.Run() ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esx5update/Cmdline.py", line 148, in Run ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esximage/Transaction.py", line 250, in InstallVibsFromSources ```

```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esximage/Transaction.py", line 356, in _installVibs ```

```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esximage/Transaction.py", line 399, in _validateAndInstallProfile ```

```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esximage/HostImage.py", line 600, in Stage ```
```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esximage/Installer/BootBankInstaller.py", line 719, in PreInstCheck ```

```2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR:   File "/build/mts/release/bora-5572656/bora/build/esx/release/vmvisor/sys-boot/lib/python2.7/site-packages/vmware/esximage/Installer/BootBankInstaller.py", line 694, in CheckInstallationSize ```

``` 2018-10-08T08:23:18Z esxupdate: 31744883: esxupdate: ERROR: InstallationError: ('', 'The pending transaction requires 242 MB free space, however the maximum supported size is 239 MB.') ```

This seems to be a known issue for Lenovo servers. It turns out they cram a whole lot of drivers in there, more than other vendors. The only known workaround is to remove VIBs from the host that are not needed. This is also described in [https://kb.vmware.com/s/article/2144200.][1]

To find out what VIBs are installed, log onto the affected host using SSH and run this command:

```esxcli software vib list```

{{< figure src="./images/vib-list.png" alt="vib list">}}

Once you have identified a driver that you want to remove, put the host in maintenance mode and this command:

```esxcli software vib remove -n qlogic-nx2-provider```

In my case I wasn't using the qlogic nx2 driver so I removed it. Now you should be able to update the host using VUM,

 [1]: https://kb.vmware.com/s/article/2144200