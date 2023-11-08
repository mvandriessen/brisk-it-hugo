---
title: Change NSX cluster certificate
author: Maarten Van Driessen
type: post
date: 
url: /change-nsx-cluster-certificate/
featured_image: 
categories:
  - NSX
  - VMware
tags:
  - nsx
  - nsx-t
  - vmware
  - api
---
One of the things you always need to do when setting up a new environment, is changing the certificates to trusted ones. This can be either from a public authority or your own internal PKI.
Usually this is pretty straight forward. For NSX however, I found that it was not so straight forward and I had to muck around with it for a bit.

