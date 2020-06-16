---
title: Cheap Kubernetes cluster for less than 15 bucks?
type:
- post
series:
- Cheap Kubernetes Cluster
tags: 
- kubernetes
- selfhosting
- devops
---
There have been several tutorials <sup>[[1]](https://www.replex.io/blog/the-ultimate-kubernetes-cost-guide-aws-vs-gce-vs-azure-vs-digital-ocean)[[2]](https://medium.com/datadriveninvestor/the-easiest-and-cheapest-way-to-try-kubernetes-for-yourself-d7367ff1d6e)[[3]](https://devonblog.com/containers/affordable-kubernetes-cluster/)</sup> online that talk about setting up your own kubernetes cluster on different cloud provider or even claim that is the cheapest. Because of my new founded interest in container orchestration, I was _keen_ on setting up my own cluster to experiment and use it as a learning opportunity.

After experimenting and tweaking few settings in GCP (free tier), and I found a way to get the _per-day_  burn-rate to 33 cents/day that is close to 10 bucks a day. I was already excited at this number --

{{< tweet 1185300426461188098 >}}

_**But**_, that was short-lived excitement. As soon as I added a public facing Loadbalancer, the rate jumped back up-to 6-7 bucks a day. This is totally the opposite of what I wanted and the rest of the cloud providers Digital Ocean, Azure etc., weren't far from that number either.

After much deliberation, I found a great other alternative. _Build my own [Bare-metal](https://en.wikipedia.org/wiki/Bare-metal_server) Kubernetes Cluster_ and I have chosen [Hetzner Cloud](https://www.hetzner.com) as my VPS provider.

In this blog series, *Cheap Kubernetes Cluster* I will explain or at least glaze through the steps I took to setup --

- **Password Manager** - think your own _lastpass_ 
- Secure and private **DNS Server stack**
- **Mail Server**
- **Object Storage** - _think your own AWS S3_
- Public facing **IP**
- **Analytics Server** (Coming 🔜!)
- Monioring/Alerting Stack
- Gitlab CI/CD Runners

.. and more!

All _these_ services with sparing 50% of the resources costing less than 15 euros. yes, no jokes! don't believe me, here's the snap of my invoices

![This is an image](/img/invoice.png)

More parts in the series coming very soon ✌️.

----
~ That's all folks ~