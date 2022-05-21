---
title: ☁️ Configuring Synology NAS to use its own domain name. Part 1
pin: true
date: 2018-11-22 05:33:24 -0400
categories: [Synology]
tags:
  - synology
---

In this post, I describe how you could configure Synology NAS to use your own domain even if you don't have static IP.

---

[Synology NAS](https://amzn.to/2S6JOfI) provides a build-in functionality to configure your domain like **[fisenko.net](https://fisenko.net)** even you don't have a static IP address. You just need to open: _Control Panel_, _External Access_, and click on _Add_ in _DDNS_ section.

![Control Panel](/assets/img/blog/synology-configuring-domain-part-1/image-1.png){: w="50%" }

It works fine until you need to add sub-domains, like _[**blog**.fisenko.net](https://blog.fisenko.net/)_, _**blahblahblah**.fisenko.page_ etc. For some reason, the build-in _DDNS_ settings don't allow to add more than one domain for one service provider. For example, I want to configure _[fisenko.net](https://fisenko.net/)_, _[blog.fisenko.net](https://blog.fisenko.net)_ with Google service provide.

Fortunately, there is awesome package **[DDNS Updater 2](https://www.cphub.net/?p=ddnsupdater2)**

> DDNS Updater 2 is a web interface and client to configure and automatically update dynamic DNS hostnames. A variety of providers can be created 'Out of the Box' by the customizable user dialogues, configurable response codes, and protocols. If a protocol still can not set up API compliant, this can be achieved through an additional module. The setup wizard is another new tool with multiple features. These range from the first analysis via functional tests of compatibility with a module up to the completion of a new provider or protocol. New providers or protocols, if the compatibility is provided with a built-in module, can be distributed by import/export function easily via the Community.

To get it worked **[DDNS Updater 2](https://www.cphub.net/?p=ddnsupdater2)** needs to be downloaded and installed after that multiple sub-domains could be configured as below

![DDNS Updater 2](/assets/img/blog/synology-configuring-domain-part-1/image-2.png){: w="50%" }
_DDNS Updater 2_

![DDNS Updater 2 domain configuration](/assets/img/blog/synology-configuring-domain-part-1/image-3.png){: w="50%" }
_DDNS Updater 2 domain configuration_

The same configuration needs to be done for all subdomains.

In my case, I use [Google Domains](https://domains.google/) since it provides Dynamic DNS support out of the box. More details about Dynamic DNS could be found on the [Google Domains Help](https://support.google.com/domains/answer/6147083?hl=en&ref_topic=9018335).

Now DNS is configured but an additional step is required to add HTTPS support. Fortunately, Synology already has built-in [Let's Encrypt](https://letsencrypt.org) support. Certificates could be added as shown below:

![Control Panel, Security, Certificates](/assets/img/blog/synology-configuring-domain-part-1/image-4.png){: w="33%" .normal }
![Add a new certificate](/assets/img/blog/synology-configuring-domain-part-1/image-5.png){: w="33%" .normal }
![Create certificate](/assets/img/blog/synology-configuring-domain-part-1/image-6.png){: w="33%" .normal }

After these simple steps domains will be available over HTTPS with a valid certificate, e.g. _[blog.fisenko.net](https://blog.fisenko.net)_ and _[fisenko.net](https://fisenko.net)._

_Update_: read [Configuring Synology NAS to use its own domain name. Part 2](https://fisenkodv.github.io/landing/posts/synology-configuring-domain-part-2/) to see how to simplify the configuration.

Thank you! 🙏🏻

## Related Posts

- [Configuring Synology NAS to use its own domain name. Part 2](https://fisenkodv.github.io/landing/posts/synology-configuring-domain-part-2/)
- [Configuring Synology NAS to use its own domain name. Part 3](https://fisenkodv.github.io/landing/posts/configuring-synology-nas-to-use-its-own-domain-name-part-3/)

## Links

- [Synology 2 bay NAS DiskStation DS218+](https://amzn.to/2Kq2J2L)
- [WD Red 4TB NAS Hard Drive](https://www.amazon.com/Red-4TB-NAS-Hard-Drive/dp/B00EHBERSE/ref=pd_bxgy_147_img_2?_encoding=UTF8&pd_rd_i=B00EHBERSE&pd_rd_r=0f380cf2-ee17-11e8-ac6d-e5cddbbd9c33&pd_rd_w=iEFGP&pd_rd_wg=f8Cbb&pf_rd_i=desktop-dp-sims&pf_rd_m=ATVPDKIKX0DER&pf_rd_p=6725dbd6-9917-451d-beba-16af7874e407&pf_rd_r=VY4PTZ4YND3B98G6CXBD&pf_rd_s=desktop-dp-sims&pf_rd_t=40701&psc=1&refRID=VY4PTZ4YND3B98G6CXBD)
