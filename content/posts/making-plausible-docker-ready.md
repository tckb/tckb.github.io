---
title: Making Plausible Analytics possible for selfhosting
description : |
   Creating a docker ready fork for Plausible Analytics
type:
- posts
- post
tags: 
- selfhosting
- analytics
keywords :  
 - 'Plausible Analytics'
 - 'selfhosting'
images:
 - https://analytics.dev.tgrthi.me/images/icon/plausible_logo-973ea42fac38d21a0a8cda9cfb9231c9.png
aliases:
  - /2020/making-plausible-docker-ready/
---
Creating a docker ready fork for Plausible Analytics
=====================

As a privacy enthusiasts and advocate, I was searching for a privacy friendly opensource analytics tool for _this_ blog. One of my other requirement was that it should be _simple_ enough for hosting on my [k8s cluster]({{< ref "posts/cheap-kubernetes-cluster.md" >}}) (more on that in later posts) and after few searches I ended up finalizing two choices:

- [Matomo](https://matomo.org/)
- [Plausible Analytics](plausible.io/)

both being opensource was a BIG plus but there are several others too, I excluded some of them as it may be too simple or does not qualify in the privacy part.

But the winner was _Plausible_ because I really like the UI and it had a single page stats that I wanted and more importantly it was written almost entirely in Elixir (one of my other interests). But the problem was that it didn't have enough support for _docker_ based hosting.

From their readme _back then_ --

>At the moment we don't provide support for easily self-hosting the code. Currently, the purpose of keeping the code open-source is to be transparent with the community about how we collect and process data.

which makes **sense** being small startup. As I acquired some experiences in field of operations, I decided to make it suitable for "self-hosting".

Initially, I had no plans to merge these changes into the upstream project and to be clear, I *do not* work on github anymore so I created a public [Gitlab fork](https://gitlab.com/tckb-public/plausible) and started working there. After the initial [announcement](https://github.com/plausible/analytics/pull/53#issuecomment-623600917), [uku](https://twitter.com/ukutaht) was [pleased](https://github.com/plausible/analytics/issues/26#issuecomment-627298735) to hear about the progress and extended his help in untangling me with possible blockers and clarifying issues that helped me in finishing my changes.

After few more discussions, the final [pull-request](https://github.com/plausible/analytics/pull/64) was merged and after some cleanups, now Plausible selfhosting beta is now open 🎉🥳

{{< tweet 1271477365730918400 >}}

and announcement on [Plausible blog](https://plausible.io/blog/self-hosted-web-analytics-beta).

There is still much work to do and needs more polishing and hence beta testers are invited. Yes, go ahead and add your issues with the hosting.

I, however, still need to do some modifications to my cluster to accommodate the newly added clickhouse into my cluster. And sadly this blog is still using  *sigh* Google Analytics (Psst.. privacy mode enabled!), once I have my setup ready, I will [de-Google-ify](https://dev.to/markosaric/how-to-de-google-ify-your-website-4bfc) this blog too ✌️!

Update :
> Plausible analytics [quoted me](https://docs.plausible.io/authors#contributors) as one their contributors. Thanks!