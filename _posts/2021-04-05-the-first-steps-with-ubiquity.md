---
title: 🐾The First Steps With Ubiquity
date: 2021-04-05 17:39:59 -0400
categories: [Ubiquity]
tags:
  - ubiquity
---

Not so long ago I was seriously puzzled by the question of a more competent organization of the local network at home. Since the number of devices is constantly increasing: phones, tablets, smart devices (Google Mini, Echo Spot, etc), laptops, IoT, TV, AV Receivers, etc. Security becomes the cornerstone and if all devices are connected to a single access point, in my case [ASUS RT-AC3200](https://www.asus.com/us/Networking/RTAC3200/) there is no distinction how each type of device could communicate with each other. For example, I don't want to give smart devices like Google Mini to get access to my NAS or even "see" the NAS. [VLANs](https://en.wikipedia.org/wiki/Virtual_LAN) for each type of device could be useful for delimitation the boundaries of the devices.

After Googling and researching I settled upon Ubiquity products. They provide plenty of options to build the network, starting from home devices and ending with more sophisticated, enterprise-level devices.

The next step was research on how to achieve what I need, i.e. delimitation the boundaries of the devices. Luckily I found an amazing [YouTube channel](https://www.youtube.com/channel/UCkLTdFxnM_0Ej163wR9wI1g/featured) and video on that channel, see the link below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-ggmjdFWfqw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Where the author describes how to organize, configure the VLANs, create the rules. His approach totally makes sense for my setup, even the handy spreadsheet to write down information about patch panel ports, VLANs, static IPs, comments, etc. You can download the spreadsheet from [here](https://www.dropbox.com/sh/bjufgi7l43z1pcw/AAAa8Jqvrx8Gbk_701emx8Bra?dl=0).

I made a decision to buy the initial set of Ubiquity devices but keeping in mind that I might need to extend that set, also I wanted to place all my stuff in a small rack, to keep them secure and well organized, that's why my choice fell on the unit models. Below is my Ubiquity devices:

- [UniFi Dream Machine Pro](https://store.ui.com/collections/unifi-network-routing-switching/products/udm-pro)
- [UniFi Switch 16 PoE](https://store.ui.com/collections/unifi-network-routing-switching/products/usw-16-poe)
- [UniFi 6 Lite Access Point](https://store.ui.com/collections/unifi-network-access-points/products/unifi-ap-6-lite)
- [Direct Attach Copper Cable, SFP+](https://store.ui.com/collections/unifi-accessories/products/unifi-sfp-dac-patch-cable)

![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/IMG_0312.jpeg){: w="70%" }
![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/IMG_0313.jpeg){: w="70%" }
![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/IMG_0309.jpeg){: w="70%" }
![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/IMG_0310.jpeg){: w="70%" }
![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/IMG_0625.jpeg){: w="70%" }

To place [Ubiquity](https://www.ui.com/) devices, [NAS](https://www.synology.com/en-us/products/DS218), [Philips Hue Bridge](https://www.philips-hue.com/en-us/p/hue-bridge/), [Raspberry Pi](https://www.raspberrypi.org/) I decided to use 9 unit rack, dedicated rack mounted power strip, patch panel. Here is the list:

- [StarTech.com 2 Post 9U 19" Wall Mount Network Cabinet](https://www.amazon.com/gp/product/B0721XSYZJ/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1)
- [Tripp Lite 14 Outlet Network-Grade Rackmount PDU](https://www.amazon.com/gp/product/B00006B83A/ref=ppx_yo_dt_b_asin_title_o04_s00?ie=UTF8&psc=1)
- [StarTech.com 2U Server Rack Shelf](https://www.amazon.com/gp/product/B008X3JHJQ/ref=ppx_yo_dt_b_asin_title_o04_s01?ie=UTF8&psc=1)
- [ETS 24-Port CAT6A FTP Shielded 1U Patch Panel](https://www.amazon.com/gp/product/B086T6KRCH/ref=ppx_yo_dt_b_asin_title_o05_s00?ie=UTF8&psc=1)
- [Ethernet cables](https://www.amazon.com/gp/product/B004C4SDO0/ref=ppx_yo_dt_b_asin_title_o04_s02?ie=UTF8&psc=1)

Below are images of what I got after assembling.

![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/device_1.jpeg){: w="70%" }
![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/device_2.jpeg){: w="70%" }
![Device Picture](/assets/img/blog/the-first-steps-with-ubiquity/device_3.jpeg){: w="70%" }

after almost one month of using the new system, I can say with confidence that it worth it, because I have more control over the system, I can better manage devices I have, I noticed bigger throughput during the day and of course, it is always fun to build something new with ability to extend in the future. So far I see the only cons is the size, definitely, it is bigger than my old ASUS RT-AC3200 😂.

### Useful Links

<iframe width="560" height="315" src="https://www.youtube.com/embed/afRV3qYuSfg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
