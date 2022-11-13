---
title: VCSA upgrade – deployment sizes missing
author: Maarten Van Driessen
type: post
date: 2020-06-11T10:59:08+00:00
url: /vcsa-upgrade-deployment-sizes-missing/
categories:
  - VMware
tags:
  - logs
  - update
  - VMware

---
Everyone has done VCSA upgrades dozens of times, but every now and then you come across something that you haven't seen before. Today, this was the case while I was doing an upgrade.

When you get to the deployment size selection, in the first stage of the upgrade, I noticed I was unable to select anything smaller than "Medium" size.
{{< figure src="./images/medium-size.png" alt="medium size">}}

Changing the storage size also didn't make the Tiny or small sizes available. While I was doing this, I happened to be talking to [Jens Herremans](https://twitter.com/HerremansJens) (Check out his [blog](https://cloud-duo.com/), it's awesome!). He told me to check the size of the logs partition on the source VM.

## Checking log sizes

After opening an SSH session to the source VM and changing to the /var/log/vmware folder, I ran the following command to get a list of all files and their sizes.

```ls -lahR  < < size.txt```

I write the output to a file to make it easier to review, also this avoids having to scroll up and having parts capped off. Next, review the file with your favorite editor. You'll see a list of all the folders, their total size and the individual files with their size. For me the big directories are vpxd, sca, sso, vdcs and vsphere-client.

Every file will also have a timestamp that makes it easy to verify if you still need the logs or not. If you wish to keep the, you can easily move them to a datastore or copy them locally. In this particular environment, we had no need for any of the older log files. I could just go in and rm -rf all the archived logs. For example in the /var/log/vmware/sca folder, the following command will remove all the archived logs but keep the one currently in use

```rm -rf sca.log.*```

# 

After the cleanup, I ran the upgrade assistant again and was able to select the tiny deployment size.
{{< figure src="./images/tiny-size.png" alt="tiny size">}}