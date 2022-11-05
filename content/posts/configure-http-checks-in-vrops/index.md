---
title: Configure HTTP checks in vROPS
author: Maarten Van Driessen
type: post
date: 2022-09-16T12:35:50+00:00
url: /configure-http-checks-in-vrops/
categories:
  - vROPS
tags:
  - http
  - VMware
  - vrops

---
This week I am playing around with vROPS. I was trying to set up an HTTP check against one of the APIs we host internally. This took me a bit longer than I would have liked. In vROPS 8, VMware switched to the telegraf agent for agent-based monitoring. It took me a while to figure out, but HTTP checks are done using the telegraf agent. Let's install the agent on one of our servers, so we can configure HTTP checks in vROPS!

## Cloud proxy

Before we can start deploying the agent, we need to deploy a cloud proxy. This cloud proxy is an appliance that handles all communication to the agents. Typically you would only need 1 cloud proxy per geo, but this can vary based on your requirements.

To deploy a cloud proxy, go to **data sources** and click on **Cloud Proxies**.

{{< figure src="./images/image.png" alt="cloud proxies">}}

On the next page, click the **New** button at the top. You will be given the link to download the Cloud proxy OVA, as well as a one-time key. You need to enter this key when deploying the ova. The key is used by the cloud proxy to authenticate to vROPS.

The ova deployment is pretty straightforward. Make sure you correctly enter the one-time key. Give the proxy a friendly name, so you can recognize it in vROPS and enter the correct network details for your environment.

After the deployment is done, it can take a few minutes before the proxy is shown in vROPS. But when it does, you should see something like this.

{{< figure src="./images/image-1.png" alt="Cloud proxy overview">}}

## Telegraf agent

The telegraf agent is an "open source, plugin-driven agent for collecting and reporting metrics". It's a small agent that you can push out to your servers using vROPS or the scripts that VMware [provides](https://docs.vmware.com/en/vRealize-Operations/8.6/com.vmware.vcom.core.doc/GUID-0610FA99-1F01-47DF-ACF7-22B74F0296E7.html). 

To start the installation, go to **Environment** and click **Applications.** On the next page, click on **Manage Telegraf agents**.

{{< figure src="./images/image-2.png" alt="Manage telegraf agents">}}

In the pane on the right, select the VM(s) to which you want the Telegraf agent to be deployed. Click on the three dots at the top and select **Install**.

{{< figure src="./images/image-3.png" alt="Install telegraf agent">}}

In the install wizard, you can use common or specific credentials for every VM you're deploying to. In my case, I'm using the same credentials for all VMs. On the next page, enter the credentials. If you're deploying to domain-joined servers, make sure the account you provide has local administrator privileges.

Next, you'll be taken to a summary page to review all the servers you're deploying to. To start the installation click on **Install agent**. vROPS will now go out and push the agent to the servers you specified. Wait a few minutes until the status is shown like in my example.
{{< figure src="./images/image-4.png" alt="Installation successful">}}

After the installation is complete, it will take a couple more minutes until the collection status is shown as green and you can start configuring the agent.

## Configuring HTTP check

Once the collection status is green, you will see an arrow in front of the VM name. Unfold the menu and click the 3 dots before HTTP check and click **Add**.

{{< figure src="./images/image-5.png" alt="Add HTTP check">}}

A new window will pop up on the right-hand side. Give your check a meaningful name. This name will be everywhere in vROPS. Enter the details of your check and click on **Save** when you're ready

{{< figure src="./images/image-7.png" alt="HTTP check">}}

Wait a few minutes for vROPS to configure the check and for a couple of polls to pass. Once the check is healthy, you'll see the icon change.

{{< figure src="./images/image-8.png" alt="HTTP Check running">}}

Now, you can just use the global search, or go to the VM object to check on your HTTP check.

{{< figure src="./images/image-9.png" alt="">}}