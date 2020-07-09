---
author :  'Chandra Tungathurthi'
date :  2020-07-01T14:57:56.375Z
title : |
 Cheap Kubernetes hosting for personal use
description : |
    This article series explains how to host kubernetes cluster in an ultra low budget of 15 euros or less for non-production loads
keywords :  
 - 'Cheap Kubernetes cluster'
aliases:
  - /2020/cheap-k8s/
tags: 
- selfhosting
- kubernetes
series:
-  Cheap kubernetes hosting 
images :
 - /img/best-price.png
---
Cheap Kubernetes Cluster
=====================
This post is the first post in my first series, where I share my experiences in setting a Kuberenetes Cluster under ultra-low budget. This is my personal cluster in which I deployed several of my productive applications.

![Budget Kubernetes Cluster for personal use; the article explains how this is possible](/img/best-price.png)
*15 bucks for a Kubernetes cluster?*

## My Adventures With Kubernetes Cluster

There have been several tutorials [\[1\]](https://www.replex.io/blog/the-ultimate-kubernetes-cost-guide-aws-vs-gce-vs-azure-vs-digital-ocean)[\[2\]](https://medium.com/datadriveninvestor/the-easiest-and-cheapest-way-to-try-kubernetes-for-yourself-d7367ff1d6e) online that talk about setting up your own Kubernetes cluster on different cloud providers. I tried to set one on [Google Cloud](https://cloud.google.com/free) as it was easy to set up and I found a tutorial on devons' blog [\[3\]](https://devonblog.com/containers/affordable-kubernetes-cluster/). From the beginning my goal was to set up the cluster as cheap as possible, preferably less than 20 euros. In this tutorial, he points out that using [preemptive VMs](https://cloud.google.com/kubernetes-engine/docs/how-to/preemptible-vms) are better and are way cheaper than the normal nodes -- this is true. A preemptive VM is a node that is available for a maximum of 24 hours and GCP does not guarantee after which. This is somewhat close to what AWS offers as Spot instances.

After adjusting few settings in GCP, I found a way to get the _per-day_ burn-rate to 33 cents. That is close to 10 bucks a day, and I was already excited at this number:


{{< tweet 1185300426461188098 >}}


_**But**_, that was short-lived excitement. As soon as I added a public facing Loadbalancer(this is much-needed to me), the rate jumped back up-to 6-7 bucks a day. This is evidently the opposite of what I wanted. The rest of the cloud providers Digital Ocean, Azure etc., weren't far from that number either.

Because of my interest in container orchestration, I was focused on setting up my own cluster. I wanted to take this opportunity to learn by maintaining the cluster running entirely by myself.

### My Experiments With Baremetal Servers for Kubernetes

After much deliberation, I found a great other alternative provider that offers cheap, yet good [Bare-metal](https://en.wikipedia.org/wiki/Bare-metal_server) servers, [Hetzner Cloud](https://www.hetzner.com/).

The cheapest VPS offering from Hetzner is their CX11 server, that offers 1vCPU and 2Gb Ram. This would be enough for working with simple load. A comparable offering in GCP would be `2x` [`g1-small`](https://cloud.google.com/compute/docs/machine-types#machine_types) instances.

For the argument sake, we will just 2 worker nodes and a master node with the same type of instance. In most cases, for simpler loads, this should be enough.

let's look at math -


| resource | # instances | cost (euros) |
|------------------- |------------- |-------------- |
| CX11 worker nodes | 2 | 5,78 |
| CX11 master node | 1 | 2,89 | | Floating IP | 1 | 1 | 
| Total (minimum) | | 9,67 |

**That** is quite tempting, I have all the required resources under 10 euros! Including a Public IP.

Since this is not a Cloud provider, we need to setup our own loadbalancer with the public IP that we get from Hetzner cloud. This is possible using [MetalLb](https://metallb.universe.tf/) (more on that in the next posts).

All the other resources needed to set up wouldn't cost a lot.

In this blog series, **_Cheap Kubernetes Cluster_** I will explain or at least glaze through the steps I took to set up --

*   **Password Manager** - think your own [LastPass](https://www.lastpass.com/de)
*   Secure and private **DNS Server stack**
*   **Mail Server**
*   **Object Storage** - _think your own_ [_AWS S3_](https://aws.amazon.com/s3/)
*   Public facing **IP**
*   [**Analytics Server**](https://plausible.io) (Coming 🔜!)
*   Monitoring/Alerting Stack
*   Gitlab CI/CD Runners

.. And more.

Using the above setup. All _these_ services with sparing 50% of the resources costing less than 15 euros. Yes, no jokes! don't believe me, here's the snap of my invoices.

![Cheap and budget Kubernetes cluster for personal use using Hetzner Cloud](/img/invoice.png)


More parts in the series coming very soon ✌️.