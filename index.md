
# MQTT2GO System Architecture
<p align="justify">
This proposal is a complete guide for an MQTT-enabled smart home setup. It was created based on the MQTT best practices from the available literature and commercially available frameworks [<a href="#ref1">1</a>, <a href="#ref2">2</a>, <a href="#ref3">3</a>]. The structure of this document is as follows: the whole description starts with a diagram of the whole system composition, followed by a description of each element. Further, there is a description of a whole system message flow, together with the initialization setup process.
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

* MQTT Incompatible devices are devices that do not adhere to the MQTT2GO standard directly, but they are still capable of MQTT communication. This means they can still be added into the MQTT2GO infrastructure, but they do need an MQTT-to-MQTT2GO intermediate unit (i.e., MQTT-to-MTT2GO translation layer).

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


# <a name="data-structure"></a>MQTT2GO Data Structure
<p align="justify">
This section is dedicated to the MQTT2GO topic naming convention. The structure is organized into subsections based on the device types to provide an easier understanding of the division and key roles of each entity inside the MQTT2GO standard.
</p>

* [General Commands and Reports](./mqtt2go-commands.md)
* [Users Management](./mqtt2go-management.md)
* [MQTT2GO Objects](./mqtt2go-objects.md)
* [MQTT2GO Controllers](./mqtt2go-controllers.md)


# <a name="requirements"></a>MQTT2GO Requirements
* [MQTT2GO Broker](./mqtt2go-broker-req.md)
* [MQTT2GO Controllers (Connected applications)](./mqtt2go-objects-req.md)
* [MQTT2GO Objects (Smart-Home devices)](./mqtt2go-controllers-req.md)

# References
<a name="ref1"></a>[1] MQTT Topics & Best Practices - MQTT Essentials: Part 5. Available from: [https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/)

<a name="ref2"></a>[2]  Understanding MQTT Topics. Available from: [http://www.steves-internet-guide.com/understanding-mqtt-topics/](http://www.steves-internet-guide.com/understanding-mqtt-topics/)

<a name="ref3"></a>[3] MQTT Topic Tree Design best practices, tips & examples. Available from: [https://pi3g.com/2019/05/29/mqtt-topic-tree-design-best-practices-tips-examples/](https://pi3g.com/2019/05/29/mqtt-topic-tree-design-best-practices-tips-examples/)