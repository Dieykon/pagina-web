---
title: 🖥 Home Temperature Monitoring
date: 2018-11-02 13:07:00 -0400
categories: [IoT]
tags: [arduino]
---

The story is about how I built a homebrew temperature/humidity monitoring system using Arduino, XBee, and Raspberry Pi.

## Part 1. Abstract

The main idea is to build a system to monitor temperature and humidity in my house and have access from a phone, tablet, laptop, etc. Additionally, I want to have historical data and show temperature, and humidity charts for the last 24 hours, month, and year, for instance.

To build the monitoring system I'm going to use [Arduino Pro Mini](https://amzn.to/2SriJVC) as the main device which will read data from [DHT22](https://amzn.to/2Jnt5lg) temperature/humidity sensor and send them by [Digi ZigBee Series 1](https://amzn.to/2Jnuet4). As a receiving device, I will use Raspberry PI 3 with connected [Digi ZigBee Series 1](https://amzn.to/2Jnuet4) but configured as the receiver.

All code is available on [GitHub](https://github.com/fisenkodv/home-temperature-monitoring), feel free to fork and use the code.

Let's get started.

## Part 2. Building Sending Device

In this section I'm going to go through the steps I did to create a device that will get temperature, and humidity data from the [DHT22](https://amzn.to/2Jnt5lg) sensor and send it to a receiver, see Part 3. Building Receiving Device section.

The final device looks as in the images below:

![IMG_2652.JPG](/assets/img/blog/home-temperature-monitoring/IMG_2652.JPG){: w="33%" .left }
![IMG_2649.JPG](/assets/img/blog/home-temperature-monitoring/IMG_2649.JPG){: w="33%" .left }
![IMG_2651.JPG](/assets/img/blog/home-temperature-monitoring/IMG_2651.JPG){: w="33%" .left }

### Part 2.1. Configuring XBee

Each [XBee](https://amzn.to/2Jnuet4) device must be configured to communicate with another [XBee](https://amzn.to/2Jnuet4) device, also please note that here I use XBee Series 1, see some details in the following posts [XBee Series 1 vs. Series 2](http://icircuit.net/xbee-series-1-vs-series-2/289), [XBee Buying Guide](https://www.sparkfun.com/pages/xbee_guide).

For the configuration, I used [XCTU](https://www.digi.com/products/xbee-rf-solutions/xctu-software/xctu) - which allows you to configure XBee modules and see received data sent by other XBee modules if you would like to debug how XBee module configuration works. To connect the XBee module via USB I used [Waveshare XBee USB Adapter](https://amzn.to/2PZZ9OF).

You could follow the following steps to set up XBee modules:

1. In XCTU click on Add device to add USB connected XBee module
