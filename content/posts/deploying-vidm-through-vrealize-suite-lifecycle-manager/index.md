---
title: Deploying vIDM through vRealize Suite Lifecycle Manager
author: Maarten Van Driessen
type: post
date: 2021-11-12T10:00:00+00:00
url: /deploying-vidm-through-vrealize-suite-lifecycle-manager/
featured_image: /wp-content/uploads/2021/11/2021-11-09-15_09_33-192.168.0.164_33899-Remote-Desktop-Connection.png
categories:
  - Homelab
  - VMware
tags:
  - homelab
  - vIDM
  - VMware
  - vrealize
  - vRSLCM

---
One of the reasons I got my homelab was to test out stuff that I don't necessarily have access to at work. Recently, vIDM has peaked my intrest so I decided to deploy it. I had already deployed vRealize Suite Lifecycle Manager so why not use that for the deploy!

## Getting everything ready

Before we can actually start installing vIDM, we need to get the binaries onto vRealize Suite Lifecycle Manager (vRSLCM). This can be done in three ways; connect your MyVMware account and download everything onto the appliance, copy the binaries to the appliance yourself through WinSCP, or connect vRSLCM to an NFS share. We'll be using the last option here.

When you first login to vRSLCM, click on the **Lifecycle Operations** card which will bring you to your environments, datacenters, etc. 

{{< figure src="./images/lcm-home.png" alt="lcm home">}}

Click on the **setting** link on the left hand side.

{{< figure src="./images/settings.png" alt="settings">}}

Then, near the bottom, click on the **Binary Mapping** card. On the net screen, just hit the **add binaries** button.

{{< figure src="./images/binarymapping.png" alt="binary mapping">}}

Hit the NFS radio button and enter the path to your NFS share. I did run into some issues here because I didn't realize my Synology also includes the volume name in the NFS share path. As soon as you hit the discover button, you'll get a list of all the binaries that vRSLCM detects. Import the ones you need, in our case, it's the OVA file for identity manager.
{{< figure src="./images/import-binary.png" alt="import binary">}}

## Creating a new environment

Now that we have the vIDM binary imported, it's time to prepare for the actual deployment. On the left hand side, click the **Create Environment** link. On the next screen, click the slide button. This will tell vRSLCM that we're going to install vIDM. You'll notice that the environment name changes to globalEnvironment.

Select a default password from your locker, or create a new one and select it. This password will be set on the default admin user in vIDM. Select the datacenter where you want to deploy vIDM to or add a vCenter connection if you haven't already done so. 

{{< figure src="./images/install-idm.png" alt="install idm">}}

On the next page, you can select what product is going to be installed. Notice that you can only select vIDM here. Select the correct version and deployment type. The only 2 deployment types you can choose are Standard and Cluster. For my lab, I'm selecting standard install.

{{< figure src="./images/new-install.png" alt="new install">}}

You'll be presented with the EULA on the next page which you'll read through entirely before accepting of course... 🙂 Now it's time to select a certificate, if you've got one imported already you can just select that one. I don't have one available yet so I will generate a new one for the lab and replace it later, once my PKI has been set up.

{{< figure src="./images/certificate-summary.png" alt="certificate summary">}}

The next page is all about the infrastructure itself, select your vCenter, datastore, network and all the usual stuff for your environment and hit next.
{{< figure src="./images/infrastructure-summary.png" alt="infrastructure summary">}}

Enter the network settings for your environment and hit the **Edit server selection** button.

{{< figure src="./images/network-info.png" alt="network info">}}

By default the same DNS servers, that are configured on the appliance will be shown. If you want to add additional ones, you can do so by clicking add new server in previous screen. Select the DNS servers you want and configure them in the correct order

{{< figure src="./images/dns.png" alt="dns">}}
{{< figure src="./images/dnsprio.png" alt="dns priority">}}
{{< figure src="./images/network-summary.png" alt="network summary">}}

The final page ties everything together and lets you set the final bits of configuration. Review that the certificate and password are the correct ones. Additionally you can select the node size here as well. I selected the medium size here, which deploys the VM with 8 CPUs and 16 GB RAM. I also entered an admin email and default username. 

At the bottom of the page, I entered the VM name, FQDN and IP address.

{{< figure src="./images/fqdn.jpg" alt="fqdn">}}

## Review and deploy vIDM

Now that all the details have been entered, you can review everything and run through the prechecks. Several checks will be done here, whether the DNS records exists or not, if the IP address is actually free, etc. Correct any errors you get and read through the warnings to make sure you can continue.

{{< figure src="./images/precheck.png" alt="precheck">}}

As soon as you hit deploy, you will be taken to the request details where you can follow along with every step of the way. I actually quite like looking at this as it looks great.
{{< figure src="./images/deploy-1.png" alt="deploy">}}
{{< figure src="./images/deploy-2.png" alt="deploy">}}
{{< figure src="./images/deploy-3.png" alt="deploy">}}

Once everything is done, you can go to the environments again. You'll see you have a completed globalenvironment with vIDM in it.

{{< figure src="./images/globalenv.png" alt="globalenvironment">}}

You can just open up a new browser window and go to the FQDN you entered during setup, you should be see the following screen and login with the credentials you set during setup.

{{< figure src="./images/vidmlogin.png" alt="vidm login">}}

That concludes this guide, I hope you found it useful! If you have any more questions or comments just hit me up on twitter or in the comments below!