---
title: 🛠 Creating a Linux service with systemd
date: 2019-12-01 20:46:48 -0400
categories: [Linux]
tags:
  - linux
  - iot
---

Oftentimes writing application there is a need to offload compute-heavy tasks to an asynchronous worker script, schedule tasks for later, or even write a daemon that listens to a socket to communicate with clients.

The cool thing is that it’s fairly easy to create a Linux service: use your favorite programming language to write a long-running program, and turn it into a service using [systemd](https://www.freedesktop.org/wiki/Software/systemd/).

> `systemd` is a suite of basic building blocks for a Linux system. It provides a system and service manager that runs as PID 1 and starts the rest of the system. `systemd` provides aggressive parallelization capabilities, uses socket and D-Bus activation for starting services, offers on-demand starting of daemons, keeps track of processes using Linux control groups, maintains mount and automount points, and implements an elaborate transactional dependency-based service control logic. `systemd` supports SysV and LSB init scripts and works as a replacement for sysvinit. Other parts include a logging daemon, utilities to control basic system configuration like the hostname, date, locale, maintain a list of logged-in users and running containers and virtual machines, system accounts, runtime directories and settings, and daemons to manage simple network configuration, network time synchronization, log forwarding, and name resolution.

The last time when I need to run such a service it was a [Python script](https://github.com/fisenkodv/home-temperature-monitoring/blob/master/src/Monitoring.Receiver/receiver.py) to read temperature and humidity data from the XBee receiver connected to Raspberry Pi via USB.

To create a `SERVICE_NAME` service with systemd the following steps would be done:

- create a unit file, e.g. `SERVICE_NAME.service`, like

```ini
[Unit]
Description=Service Descrioption

[Service]
WorkingDirectory=/home/<user>/<directory>
ExecStart=<command to run>
Restart=always
RestartSec=10
SyslogIdentifier=<identifier>
User=<user>

[Install]
WantedBy=multi-user.target
```

**Note** : A real unit file example for [_Home Temperature Monitoring_](https://github.com/fisenkodv/home-temperature-monitoring) could be found [here](https://github.com/fisenkodv/home-temperature-monitoring/blob/master/config/monitoring.service).

- copy `SERVICE_NAME.service` to `/etc/systemd/system/`
- make sure the script is executable `chmod +x /etc/systemd/system/SERVICE_NAME.service`
- enable the service `sudo systemctl enable SERVICE_NAME.service`
- start the service `sudo systemctl start SERVICE_NAME.service`
- check the service's status `sudo systemctl status SERVICE_NAME.service`

In my case I've used the minimum systemd features, more details about unit files could be found here [https://www.freedesktop.org/software/systemd/man/systemd.unit.html](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)
