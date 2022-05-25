---
title: 🔌 Adding RAM to Synology DS 218+
date: 2019-11-09 20:58:52 -0400
categories: [Synology, Tuning]
tags:
  - synology
---

Recently I've faced the limitation of 2GB of RAM when I wanted to run [GitLab Docker](https://hub.docker.com/r/gitlab/gitlab-ce/) container on Synology DS 218+. By default, [GitLab](https://about.gitlab.com) [recommends](https://docs.gitlab.com/ee/install/requirements.html#memory) having at least 8GB to run the container. But Synology DS 218+ has only 2 GB. The same limitation I would face if I need multiple containers even if they are small the total amount of memory required to keep them up and running exceeds 2 GB.

Fortunately, it's possible to extract, replace, and upgrade the RAM inside the Synology DS218+. The process only requires a few steps.

In my case, I extended the 2GB of system memory all the way up to 10GB.

## Below are the steps of how to upgrade Synology DS 218+ RAM

1. Shutdown the NAS.
2. Take out the two **hard drive bays**.
3. The free RAM module slot is located on the side where the power button is.
4. Take out the newly purchased **RAM module** from its packaging. I used [_Crucial 8GB Single DDR3/DDR3L 1600 MT/S (PC3-12800)_](https://www.amazon.com/gp/product/B006YG8X9Y/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)_._
5. Carefully insert the module into the slot, making sure it's the right way up. Match the notch to that on the module.
6. Re-insert the **hard drive bays**.
7. Turn on the NAS.

**Note** : some guides recommend to use _[Synology Assistant](https://www.synology.com/en-us/support/download/DS218+#utilities)_ to check the memory, I didn't do that luckily, everything works without any problems

After those simple steps, you will be able to take full advantage of 10GB of RAM. The official word from Synology is only a 4GB module can be installed (for a total of 6GB) but 10GB seems to work just fine.

![Resource Monitor](/assets/img/blog/adding-ram-to-synology-ds-218/resource-monitor.png){: w="70%" }
_Resource Monitor_

If you want to go all out and hit 16GB of RAM, it's possible to replace the original 2GB module, but you need to take apart the NAS completely and isn't recommended unless you truly need extra memory.

![DS 218+](/assets/img/blog/adding-ram-to-synology-ds-218/IMG_6138.jpeg){: w="70%" }
![DS 218+](/assets/img/blog/adding-ram-to-synology-ds-218/IMG_6137.jpeg){: w="70%" }
![DS 218+](/assets/img/blog/adding-ram-to-synology-ds-218/IMG_6139.jpeg){: w="70%" }
![DS 218+](/assets/img/blog/adding-ram-to-synology-ds-218/IMG_6140.jpeg){: w="70%" }
