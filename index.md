# Release Versions

| Date | Revision | Description | Changelog |
| ---- | -------- | ----------- | --------- |
|<img width=200/>|<img width=200/>|<img width=500/>|<img width=200/>|
| 2019-11-21 | Rev 1.1 | Updated version of the MQTT2GO standard | [Changelog](./changelog.md) |
| 2019-11-13 | [Rev 1.0](./1.0/index.md) | Initial version of the MQTT2GO standard | [Changelog](./1.0/changelog.md) |

# The MQTT2GO Project Introduction
<p align="justify"> When talking about the Internet of Things (IoT), communication is always mentioned in the first place. Interaction between sensors, devices, gateways, and user applications is the essential characteristic that creates the IoT landscape. But what enables all this smart stuff to talk and interact? To exchange the data, the IoT protocols are those enablers which can be seen as languages that the IoT devices use in order to communicate. Out of these transferred pieces of data, useful information can be extracted for the customers and thanks to it, the whole deployment turns into the working ecosystem.</p>

<p align="justify">
Nevertheless, the wide variety of currently available IoT protocols leads us to the point the Internet of Things needs standardized IoT protocols. They help to avoid further fragmentation of the market, thus minimizing the risk of the incompatibility and security threats. While it seems that this is an statement which all agree on, few efforts have been made so far to propose a world-wide standard that would unify all IoT communication.</p>

<p align="justify">
Therefore, the MQTT2GO main goal of the MQTT2GO project is to investigate one of the most emerging Internet of Things (IoT) protocols i.e., Message Queuing Telemetry Transport (MQTT) for Smart Home scenarios in which all the communication is done using the protocol in question. Based on the key findings gained during the thorough market overview, the introduced MQTT-enabled proof-of-concept demonstrator enables to handle both the MQTT devices (i.e. sensors) and MQTT controllers, see the figure "Architecture of MQTT2GO System." As the MQTT protocol provides a lightweight method of carrying out messaging using a publish / subscribe model, it is well suited for IoT messaging such as with low power sensors or mobile devices such as smartphones, embedded devices, home dashboards, etc.</p>

<p align="justify">
The introduced MQTT2GO standard manages to handle also the non-MQTT devices, which can be also part of the Smart Home scenario. Relying on the current ICT trends, the MQTT2GO standard introduces two MQTT brokers, which are located in the local network (local MQTT broker) and in the remote infrastructure of the service provider (cloud MQTT broker) respectively. This innovative concept opens the door to a wide variety of communication scenarios e.g., the configuration of the new smart home sensor can be done via the smartphone, which is even not present in the local WLAN. Also, the MQTT2GO brings to the light the security aspects of the wired/wireless communications and introduces innovative principles that are technology-driven but at the same time easy to use for the customers.</p>

<p align="justify">
The lessons learned gathered during the project development are further revealed in the form of the MQTT2GO requirements for vendors and/or providers of all the components within the Smart Home scenario i.e., MQTT brokers, MQTT controllers (connected applications), and MQTT objects (Smart Home devices). Therefore, the provided information should guide the potential 3rd party developers / customers to easily reach the desired implementation level to get their products compatible with the introduced MQTT2GO standard.</p>

# MQTT2GO System Architecture
<p align="justify">
This proposal is a complete guide for an MQTT-enabled smart home setup. It was created based on the MQTT best practices from the available literature and commercially available frameworks [<a href="#ref1">1</a>, <a href="#ref2">2</a>, <a href="#ref3">3</a>]. The structure of this document is as follows: the whole description starts with a diagram of the whole system composition, followed by a description of each element. Further, the document contains thorough description of MQTT2GO standard message flow, together with the process of adding new device.
</p>

<p align="center" >
	<img src="mqtt_architecture.svg" alt="Architecture of MQTT2GO System.">
</p>
<p align="center" >
	<em>Architecture of MQTT2GO System.</em>
</p>

## Multipurpose Smart Home Enabler
<p align="justify">
This section introduces the MQTT2GO standard architecture. The two main parts are the MQTT cloud broker and MQTT2GO local broker. The latter is the core “brain” of the whole MQTT2GO standard providing functions for user management, accounting and overall administration done by the service provider. This part is mentioned in this project solely for the purposes of direct MQTT communication between the end devices and the aforementioned cloud broker. Also, as the MQTT2GO standard is targeted to data exchange inside a smart home/household, the business logic included inside the cloud broker is not covered. Aside of that, most of the traffic is planned to go through a local MQTT broker, which is the primary “gateway” for all household devices. This secures the internet-independency that offers users more flexibility and higher service availability even in outage situations.
</p>

<p align="justify">
The whole architecture is having an onion-like structure, where the topmost layer is the MQTT Cloud broker, which is “supervising” whole MQTT2GO standard ecosystem. Then, the next layer is the Smart Home itself, where the Smart Home Gateway (SH-GW) and all devices reside. The smart home doesn’t have to be composed of only one SH-GW placed in one location - there can be multiple physical locations and SH-GWs supervised by one “family” (group of users). Also even though the ideal case is aiming at usage of the SH-GW as a primary “relay” for all connected devices, if the end device is MQTT-enabled, it can be directly connected to the MQTT Cloud Broker. This option is primarily targeted at users, who do not want to own a physical (local) SH-GW, but still want to take advantage of the MQTT2GO standard.
</p>

<p align="justify">
The connected devices themselves can be any smart object that is able to communicate via a standardized technology with the local or cloud MQTT broker. The only difference here is in the final implementation, where the MQTT2GO compliant devices can “talk” directly to the MQTT brokers and the rest of the devices need to utilize an intermediate device/endpoint to translate their standard into the correct MQTT2GO compatible message structure.
The last part is the controlling applications, which can be run on any platform. This enables developers and users to utilize their own devices/software to control the MQTT2GO smart home if they adhere to the MQTT2GO standard.
</p>

### MQTT Devices
<p align="justify">
The term MQTT devices inside the MQTT2GO standard is used to describe any MQTT-capable device. This means that if the device is capable of communication via the MQTT standard, it belongs to the MQTT Devices group. They can be further divided into two groups:
</p>

* MQTT2GO Compatible devices, which adhere to the MQTT2GO standard and therefore can communicate directly with the MQTT2GO gateway or backend without intermediate MQTT translating broker.

* MQTT2GO Non-compliant devices are devices that do not adhere to the MQTT2GO standard directly, but they are still capable of MQTT communication. This means they can still be added into the MQTT2GO infrastructure, but they do need an MQTT-to-MQTT2GO intermediate unit (i.e., MQTT-to-MTT2GO translation layer).

### Non-MQTT Devices
<p align="justify">
The non-MQTT devices are all devices working on any other standards than MQTT. Hereby, all Zigbee, Z-Wave, Bluetooth, Wi-Fi, etc. devices are part of this group.
</p>

### MQTT Controllers
<p align="justify">
MQTT2GO standard defines a special group of devices, called MQTT Controllers. These special devices are used to control a part of the MQTT2GO ecosystem or devices, respectively. The reason why they are mentioned here is the fact, that their purpose is vastly different from the other devices, which are in most situations, pure subscribers/listeners.
</p>


#  <a name="add-devices"></a>Process of Adding New Devices
In this chapter, we are going to present few ways how to add a new device. Firstly, the ideal MQTT2GO-preferred process is described. Then the other ways of adding devices are introduced.

* [Setup via Guest WiFi](./add-wifi.md)
* [Setup via WPS](./add-wps.md)
* [Setup via Remote MQTT Broker](./mqtt2go-remote-broker.md)
* [Setup of New MQTT2GO Non-Compliant Devices](./add-non-compliant.md)
* [Setup of New MQTT2GO Controller App](./add-controller.md)


# <a name="data-structure"></a>MQTT2GO Data Structure
<p align="justify">
This section is dedicated to the MQTT2GO topic naming convention. The structure is organized into subsections based on the device types to provide an easier understanding of the division and key roles of each entity inside the MQTT2GO standard.
</p>

* [General Commands and Reports](./mqtt2go-commands.md)
* [Users Management](./mqtt2go-management.md)
* [MQTT2GO Objects](./mqtt2go-objects.md)
* [MQTT2GO Controllers](./mqtt2go-controllers.md)


# <a name="requirements"></a>MQTT2GO Requirements
This section is related to the requirements which have to be fulfilled in order to work with the proposed MQTT2GO standard. The given requirements do cover the key functionalities which the MQTT2GO takes into the account as the basic features of the communication system.
* [MQTT2GO General Requirements](./mqtt2go-general-req.md)


# References
<a name="ref1"></a>[1] MQTT Topics & Best Practices - MQTT Essentials: Part 5. Available from: [https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/)

<a name="ref2"></a>[2]  Understanding MQTT Topics. Available from: [http://www.steves-internet-guide.com/understanding-mqtt-topics/](http://www.steves-internet-guide.com/understanding-mqtt-topics/)

<a name="ref3"></a>[3] MQTT Topic Tree Design best practices, tips & examples. Available from: [https://pi3g.com/2019/05/29/mqtt-topic-tree-design-best-practices-tips-examples/](https://pi3g.com/2019/05/29/mqtt-topic-tree-design-best-practices-tips-examples/)
