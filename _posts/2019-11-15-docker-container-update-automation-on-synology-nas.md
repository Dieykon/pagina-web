---
title: 🏗️ Docker container update automation on Synology NAS
date: 2019-11-15 01:20:00 -0400
categories: [Synology]
tags:
  - synology
  - docker
---

Using Docker on Synology NAS is quite straightforward. Deploying a new container comes down to a few simple steps: download an image, launch with required parameters and that is it. Updating also straightforward, but every time the same steps need to be done. Especially tiresome when a container has a set of environment variables and volume mappings and especially when there are bunch of such containers.

Fortunately, there is a simple and elegant way of how to automate the containers update process using [Watchtower](https://github.com/containrrr/watchtower).

> Watchtower is an application that will monitor your running Docker containers and watch for changes to the images that those containers were originally started from. If watchtower detects that an image has changed, it will automatically restart the container using the new image. With watchtower you can update the running version of your containerized app simply by pushing a new image to the Docker Hub or your own image registry. Watchtower will pull down your new image, gracefully shut down your existing container and restart it with the same options that were used when it was deployed initially.

The most important thing is the ability to run [Watchtower](https://github.com/containrrr/watchtower) as a Docker container on NAS. The Docker image for Watchtower is available on the Docker Hub and has [comprehensive documentation](https://hub.docker.com/r/v2tec/watchtower/).

To run the Watchtower on the NAS there is on a small trick than needs to be done, point is that the path to `docker.sock` should be passed as a volume setting, but Synology Docker package doesn't allow to configure such path. As a workaround I did the following steps:

- Enabled SSH connections
- Connected to the NAS via SSH
- Ran the Watchtower on the NAS, like `sudo docker run -d --name watchtower -v /var/run/docker.sock:/var/run/docker.sock v2tec/watchtower`
- Disabled SSH connections

After those simple steps, the Watchtower works and Synology Docker package shows it.

![Watchtower container](/assets/img/blog/docker-container-update-automation-on-synology-nas/watchtower-container.jpg){: w="70%" }
![Watchtower logs](/assets/img/blog/docker-container-update-automation-on-synology-nas/watchtower-logs.png){: w="70%" }
