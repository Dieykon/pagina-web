---
title: 🔨Configuring Synology NAS to use its own domain name. Part 3
pin: true
date: 2021-06-09 15:13:19 -0400
categories: [Synology, Configuration]
tags:
  - synology
---

In my previous two posts "[Configuring Synology NAS to use own domain name. Part 1]({% post_url 2018-11-22-synology-configuring-domain-part-1 %})" and "[Configuring Synology NAS to use its own domain name. Part 2]({% post_url 2021-01-04-synology-configuring-domain-part-2 %})" I described configuring [DSM](https://www.synology.com/en-us/beta/DSM70Beta) to update Dynamic DNS in the [Google Domains](https://domains.google.com/). Literally, until today, namely, before I updated DSM to version 7, everything worked fine. But after the update to version 7 [DDNS Updater 2](https://www.cphub.net/?p=ddnsupdater2) stopped working due to incompatibility with the latest version. Unfortunately, I wasn't able to get the source code of [DDNS Updater 2](https://www.cphub.net/?p=ddnsupdater2) to have a chance to modify it and had to come up with a solution of how to live without that awesome package.

For some reason, Synology still has a weird implementation of DDNS in the settings, i.e. there is no way to provide a wildcard in the domain name, due to validation error, a custom provider didn't work for me too as well as it complains to add more than two same providers.

Without hesitation, I decided to go in a different way. Since Google Domains provide [an API](https://support.google.com/domains/answer/6147083?hl=en) I've created a bash script to update the IPs and configured a scheduler to run the job each hour.

```bash
#!/usr/bin/env bash

WILDCARD_DOMAIN=*.fisenko.net
WILDCARD_USERNAME=FROM_GOOGLE_DOMAINS
WILDCARD_PASSWORD=FROM_GOOGLE_DOMAINS

ROOT_DOMAIN=fisenko.net
ROOT_USERNAME=FROM_GOOGLE_DOMAINS
ROOT_PASSWORD=FROM_GOOGLE_DOMAINS

CURRENT_IP=$(curl -s ipecho.net/plain | cut -d '%' -f1)

function update() {
  echo "https://${1}:${2}@domains.google.com/nic/update?hostname=${3}&myip=${CURRENT_IP}"
  curl --location --request GET "https://${1}:${2}@domains.google.com/nic/update?hostname=${3}&myip=${CURRENT_IP}"
}

update ${WILDCARD_USERNAME} ${WILDCARD_PASSWORD} ${WILDCARD_DOMAIN}
update ${ROOT_USERNAME} ${ROOT_PASSWORD} ${ROOT_DOMAIN}
```

Below an example of my scheduler for that.

![Configuration](/assets/img/blog/synology-configuring-domain-part-3/configuration.gif){: w="70%" }

I know this is not the perfect solution since I have to modify the script manually each time I need to make a modification, but such changes are very rare it should be fine, at least for now until I have a better solution.

Hopefully, you will find it helpful 🙏🏻

### Related Posts

- [Configuring Synology NAS to use its own domain name. Part 1]({% post_url 2018-11-22-synology-configuring-domain-part-1 %})
- [Configuring Synology NAS to use its own domain name. Part 2]({% post_url 2021-01-04-synology-configuring-domain-part-2 %})
