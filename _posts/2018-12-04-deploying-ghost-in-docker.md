---
title: 🚀 Deploying Ghost to Synology NAS
date: 2018-12-04 04:02:54 -0400
categories: [Synology]
tags:
  - synology
  - docker
  - mariadb
---

Previously I described how to [configure Synology NAS to use own domain name]({% post_url 2018-11-22-synology-configuring-domain-part-1 %}). In this post, I'm going to describe how to deploy [Ghost](https://ghost.org/) in Synology NAS using [Docker](https://www.docker.com/).

> **Ghost** is a free and open-source blogging platform written in JavaScript and distributed under the MIT License, designed to simplify the process of online publishing for individual bloggers as well as online publications.
> **Docker** is a computer program that performs operating-system-level virtualization, also known as "containerization". It was first released in 2013 and is developed by Docker, Inc. Docker is used to running software packages called "containers".

All the steps below are performed on [Synology DS218+](https://amzn.to/2Ssbmg2)

---

First of all, we need to install the _Docker_ package on the NAS. It doesn't require a special configuration, just go to the _Package Center_, search _Docker_, and install it. That is it.

![Docker Package](/assets/img/blog/deploying-ghost-in-docker/image-11.png){: w="70%" }
_Docker Package_

Then I'd strongly recommend installing _[MariaDB](https://mariadb.org/)10_ package. Of course, _Ghost_ could use [SQLite](https://www.sqlite.org/)as the _DB_, but I'm going to use _MariaDB._

![MariaDB 10 Package](/assets/img/blog/deploying-ghost-in-docker/image-12.png){: w="70%" }
_MariaDB 10 Package_

There is a [_phpMyAdmin_](https://www.phpmyadmin.net/)package which helps manage installed MariaDB databases, users, and other stuff related to the database. So, I'd recommend installing that package as well.

I've created a new `blog` database in _phpMyAdmin_ as well as a separate user for the database*.*

![phpMyAdmin](/assets/img/blog/deploying-ghost-in-docker/image-13.png){: w="70%" }
_phpMyAdmin_

After that, we are good to go to deploy Ghost in the Docker.

In the _Docker_ package in the _Registry_, search and download `ghost` image

![Docker](/assets/img/blog/deploying-ghost-in-docker/image-15.png){: w="70%" }

after that, the image will be available in the _Images,_ where the `ghost` image should be selected and launched, as on the screenshot below

![phpMyAdmin](/assets/img/blog/deploying-ghost-in-docker/image-13.png){: w="70%" }
_phpMyAdmin_

![Launch Ghost image](/assets/img/blog/deploying-ghost-in-docker/image-16.png){: w="70%" }
_Launch Ghost image_

Below I'll provide the settings for the container I used

![General Settings](/assets/img/blog/deploying-ghost-in-docker/image-17.png){: w="70%" }
_General Settings_

I use _Low CPU Priority_ setting along with _256Mb Memory Limit_

![Advanced Settings](/assets/img/blog/deploying-ghost-in-docker/image-18.png){: w="70%" }
_Advanced Settings_

![Volume Settings](/assets/img/blog/deploying-ghost-in-docker/image-19.png){: w="70%" }
_Volume Settings_

Prior the _Volume_ configuration `docker/personal/ghost` should be created on the storage, &nbsp;it will allow the docker container write data to the storage through `/var/ghost/content` directory, i.e. save changes like a theme, images when the container is restarted.

![Port Setting](/assets/img/blog/deploying-ghost-in-docker/image-20.png){: w="70%" }
_Port Setting_

Any unused NAS local port could be used for the _Port Settings._ The most important part is _Environment_, see the values I used to make it works.

![Environment Settings](/assets/img/blog/deploying-ghost-in-docker/image-21.png){: w="70%" }
_Environment Settings_

After applying these steps Ghost container should be up and running.

![Running Docker](/assets/img/blog/deploying-ghost-in-docker/image-22.png){: w="70%" }
_Running Docker_

And available locally e.g. by local IP address _http://192.168.1.2:8080_. To make the blog available outside and have a domain name like [https://blog.fisenko.net](https://blog.fisenko.net) (also see _[Configuring Synology NAS to use own domain name]({% post_url 2018-11-22-synology-configuring-domain-part-1 %})_ go to the _Control Panel_, _Application Portal_, _Reverse Proxy_ and create a configuration for _HTTP_ and _HTTPS_ protocols:

![HTTP Configuration](/assets/img/blog/deploying-ghost-in-docker/image-23.png){: w="70%" }
_HTTP Configuration_
![HTTP Configuration](/assets/img/blog/deploying-ghost-in-docker/image-24.png){: w="70%" }
_HTTP Configuration_

After these manipulations, the blog will be accessible from the Internet.

Hopefully, you will find these instructions useful. Thank you! 🙏

## Updates

_09/07/2020_: _\*.fisenko.page_ domains on all screenshots could be interpreted as _\*.fisenko.net_ domains due to migration to the _.net_ domain.
