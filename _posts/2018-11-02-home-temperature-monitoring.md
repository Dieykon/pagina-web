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

![Device](/assets/img/blog/home-temperature-monitoring/IMG_2652.JPG){: w="33%" .normal }
![Device](/assets/img/blog/home-temperature-monitoring/IMG_2649.JPG){: w="33%" .normal }
![Device](/assets/img/blog/home-temperature-monitoring/IMG_2651.JPG){: w="33%" .normal }

### Part 2.1. Configuring XBee

Each [XBee](https://amzn.to/2Jnuet4) device must be configured to communicate with another [XBee](https://amzn.to/2Jnuet4) device, also please note that here I use XBee Series 1, see some details in the following posts [XBee Series 1 vs. Series 2](http://icircuit.net/xbee-series-1-vs-series-2/289), [XBee Buying Guide](https://www.sparkfun.com/pages/xbee_guide).

For the configuration, I used [XCTU](https://www.digi.com/products/xbee-rf-solutions/xctu-software/xctu) - which allows you to configure XBee modules and see received data sent by other XBee modules if you would like to debug how XBee module configuration works. To connect the XBee module via USB I used [Waveshare XBee USB Adapter](https://amzn.to/2PZZ9OF).

You could follow the following steps to set up XBee modules:

1. In XCTU click on Add device to add USB connected XBee module
   ![XCTU Main Screen](/assets/img/blog/home-temperature-monitoring/Capture.PNG){: w="50%" .normal }
   **XCTU Main Screen.**

2. Select the connected device, as on the screenshot below.
   ![Add module](/assets/img/blog/home-temperature-monitoring/Capture1.PNG){: w="50%" .normal }
   **Add module dialog.**

**Note**: if you don't see your XBee module, make sure you have installed a driver.
If you use Windows you could see the connected device in the Device Manager. I
had such an issue and after installing that particular driver, the issue has
been resolved.

After that, you will see the connected XBee module in the list of modules

![Connected XBee](/assets/img/blog/home-temperature-monitoring/Capture2.PNG){: w="50%" }
_Connected XBee_

3. Select the module and update: _ID_, _DL_, and _MY_ values.
![Select the module and update](/assets/img/blog/home-temperature-monitoring/Capture3-1.PNG){: w="50%" .normal }

Here is what I used for my configuration:

| Device   | **ID** PAN ID | **DL** Destination Address Low | **MY** 16-bit Source Address |
| :------- | :-----------: | :----------------------------: | :--------------------------: |
| Receiver |     3400      |              3501              |             3500             |
| Sender 1 |     3400      |              3500              |             3501             |
| Sender 2 |     3400      |              3500              |             3502             |
| Sender 3 |     3400      |              3500              |             3503             |

**PAN ID** must be the same for all devices, **DL** for _Sender [x]_'s is _3500_ as the
_Receiver_'s **MY 16-bit Source Address** value. Senders **MY 16-bit Source Address** just
unique IDs.

After these steps, XBee modules are ready for the next steps.

Useful links:

1. (Exploring XBees and XCTU) [https://learn.sparkfun.com/tutorials/exploring-xbees-and-xctu/all]
2. (XBee WiFi Hookup Guide) [https://learn.sparkfun.com/tutorials/xbee-wifi-hookup-guide/all]

## Part 2.2. Configuring Arduino

(DHT22) [https://amzn.to/2Jnt5lg] and (Digi ZigBee Series 1) [https://amzn.to/2Jnuet4] need to be connected to (Arduino Pro Mini) [https://amzn.to/2SriJVC] before uploading the code. See the schema below how I connected (Arduino Pro Mini) [https://amzn.to/2SriJVC] with (DHT22)[https://amzn.to/2Jnt5lg] and (Digi ZigBee Series 1) [https://amzn.to/2Jnuet4]:

(Arduino Pro Mini) [https://amzn.to/2SriJVC] pinout is available (here) [https://blog.fisenko.page/content/images/2018/10/promini.png].

I used an additional (Breakout Board for XBee Module) [https://amzn.to/2ABo9q6] with (Generic 2mm 10 Pin XBee Socket Header) [https://amzn.to/2Oe6ZCx] to connect (Digi ZigBee Series 1) [https://amzn.to/2Jnuet4].

When everything is connected code can be uploaded to the (Arduino Pro Mini) [https://amzn.to/2SriJVC]. (Arduino Pro Mini) [https://amzn.to/2SriJVC] doesn't have a USB port you need to use _FTDI_ device, I used (SparkFun FTDI Basic Breakout - 3.3V) [https://amzn.to/2COER73].

**Please note**: that I used (3.3V Arduino Pro Mini) [https://amzn.to/2SriJVC] and therefore you have to use (3.3V FTDI) [https://amzn.to/2COER73].

Arduino sketch file is available here - (Monitoring.Sender.ino) [http://bit.ly/2PBwwKU], you just need to provide a unique `DEVICE_ID` for your device, e.g. `GUID` in (line #4) [https://github.com/fisenkodv/home-temperature-monitoring/blob/c4d7f451cd9978cb36bf8f81eaf79f884c52691a/src/Monitoring.Sender/Monitoring.Sender.ino#L4] of the sketch file and upload to (Arduino Pro Mini) [https://amzn.to/2SriJVC].

Probably when you upload the code into your Arduino you will need to disconnect the XBee module, otherwise, you will get an error in the (Arduino IDE) [https://www.arduino.cc/en/Main/Software].

## Part 3. Building Receiving Device

### Part 3.1. Configuring Raspberry PI

Before running any code on Raspberry PI, you need to connect Digi ZigBee Series 1) [https://amzn.to/2Jnuet4]. In my case, I used the (XBee USB Adapter) [https://amzn.to/2PZZ9OF] for it.

Now you are ready to upload the code into your Raspberry PI. The easiest way is to use SSH. _In this post, I'm not going to describe how to install Raspbian and configure SSH, but you could easily google it._

Software for the Raspberry PI(receiver) consist from two parts:

- A (Python script) [https://github.com/fisenkodv/home-temperature-monitoring/tree/master/src/Monitoring.Receiver]
  which reads data from the USB connected (XBee) [https://amzn.to/2Jnuet4] module and calls (Web API) [https://github.com/fisenkodv/home-temperature-monitoring/tree/master/src/Monitoring.Web] to send received data;
- A (Web part) [https://github.com/fisenkodv/home-temperature-monitoring/tree/master/src/Monitoring.Web] consists from an (API) [https://github.com/fisenkodv/home-temperature-monitoring/tree/master/src/Monitoring.Web] and (UI) [https://github.com/fisenkodv/home-temperature-monitoring/tree/master/src/Monitoring.Client]
  - the _API_:
    - receives requests from the Python script and save into **MariaDB**
    - retrieve data for the UI Spa application
  - the _UI_ is the Angular application:
    - shows connected devices and theirs temperature, humidity, and (head index) [https://en.wikipedia.org/wiki/Heat_index];
    - shows historical data for each particular device using charts

To build and deploy the code to the Raspberry PI use scripts from the scripts directory: `build.sh` to build API and UI, `deploy.sh` to deploy(don't forget to update the IP address in the `deploy.sh` script).

You could call `sh ./build.sh && sh ./deploy.sh` from the scripts directory to build and deploy.

**Prerequisites** to run the code on the Raspberry PI:

- Python 3 must be installed;
- .NET Core must be install, see _Link #4_ in the Links section.

After deployment, the code connects to your Raspberry PI, navigates to the `monitoring` directory and call `nohup sh ./run.sh &` to leave `run.sh` the script running when you close SSH connection.

## Part 4. Result

The final result is a Web Application that looks like

![Main screen](/assets/img/blog/home-temperature-monitoring/main_screen.png){: w="50%" .normal }
**Main screen.**

Feel free to ask me any questions.

## Links

1. (Low Power Arduino! Deep Sleep Tutorial) [http://bit.ly/2EGqZy6]
2. (ATMEL 8-BIT MICROCONTROLLER WITH 4/8/16/32KBYTES. IN-SYSTEM PROGRAMMABLE FLASH DATASHEET) [http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-8271-8-bit-AVR-Microcontroller-ATmega48A-48PA-88A-88PA-168A-168PA-328-328P_datasheet_Complete.pdf]
3. (Setting up Raspian and .NET Core 2.0 on a Raspberry Pi) [https://blogs.msdn.microsoft.com/david/2017/07/20/setting_up_raspian_and_dotnet_core_2_0_on_a_raspberry_pi/]
4. ().NET Core on Raspberry Pi) [https://github.com/dotnet/core/blob/master/samples/RaspberryPiInstructions.md]
