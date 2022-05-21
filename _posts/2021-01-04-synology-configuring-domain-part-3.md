---
title: 🕸 Configuring Synology NAS to use its own domain name. Part 2
pin: true
date: 2021-01-04 01:43:42 -0400
categories: [Synology]
tags:
  - synology
---

In my [previous post](./synology-configuring-domain/), I described how to configure [DDNS Updater 2](https://www.cphub.net/?p=ddnsupdater2) and [Google Domains](https://domains.google/) to use Dynamic DNS. Enough time has passed since then and I had to add many subdomains for my [fisenko.net](https://fisenko.net) domain, each time adding a new subdomain lead me to the following steps:

- create a new domain configuration on the _Google Domains_
- add configuration to the _DDNS Updater 2_
- configure the reverse proxy

and each time when I need to for example rename the subdomain I had to do the same. Which is absolutely crazy and obviously not too convenient.

In one of the comments in the previous post, **[Metalheadkicks](https://disqus.com/by/metalheadkicks/)** left a [comment](./synology-configuring-domain/#comment-4948366501) about that wildcard, which could really simplify configuration and support. **[Metalheadkicks](https://disqus.com/by/metalheadkicks/)** thank you so much, I really appreciate it!

Until today I have been postponing changing the configuration, but today I finally have found a bit of time to switch the configuration to wildcards.

After the changes I did:

- disabled all previous _DDNS Updater 2_ configurations. Configurations could be removed as well if you are 100% confident.

![DDNS Updater 2](/assets/img/blog/synology-configuring-domain-part-2/image.png){: w="50%" }
_DDNS Updater 2_

- added a new configuration using a wildcard _`*.fisenko.net`, see the screenshot above_
- added a wildcard synthetic record in the _Google Domains_

![Google Domains](/assets/img/blog/synology-configuring-domain-part-2/image-1.png){: w="50%" }
_Google Domains_

I got a fully functional configuration without tons of subdomains in the _DDNS Updater 2_ and _Google Domains._

If you read the post then that approach works fine 😉

Thank you! 🙏🏻

### Related Posts

- [Configuring Synology NAS to use its own domain name. Part 1](./synology-configuring-domain-part-1/)
- [Configuring Synology NAS to use its own domain name. Part 3](./configuring-synology-nas-to-use-its-own-domain-name-part-3/)
