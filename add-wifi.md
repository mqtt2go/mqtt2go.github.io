[Back](./index.md#add-devices)
# Setup via Guest WiFi
<p align="justify" >
The ideal process of adding new device should be considered as the right way how to go through this process. Keeping this in mind, some of the steps detailed below can be reduced using a specific technology that can provide the needed functionality (i.e., WPS). The idealprocess to add a new device is based on the following steps.
</p>

## Setup Steps

1.	MQTT Controller (Mobile/Web App) initiates the process of adding a new device by subscribing to __/\<home_id\>/\<gw_id\>/add_device__. Then it publishes an activation request containing __activation code__ and __device id__. These codes can be found on the newly installed device in the form number or QR code.
2.	In response to the request, SH-GW enables the Guest Wi-Fi and sets the password inputted as an activation code.
3.	MQTT end device then connects to the Guest Wi-Fi and further utilizes mDNS (multicast DNS) to resolve address __MQTT2GO.local__, which equals to the address of the MQTT broker.
4.	The MQTT end device then connects to the initialization MQTT broker. The MQTT end device contains a pre-loaded certificate of trustworthy CA (Certification Authority), which is used to establish TLS (Transport Layer Security) communication with the MQTT broker. The chain of trust must be on both sides. Thus, the SH-GW (MQTT broker) has to contain a certificate issued by the same CA as it is contained in the MQTT end-device. If these certificates do not match, the TLS communication cannot be established.
5.	When the TLS communication is established the MQTT end device subscribes to the __\<activation_code\>/activation__ topic and publishes _GET_CREDENTIALS_ request.
6.	As a response, SH-GW (MQTT broker) generates a new set of certificates that will be used for ongoing communication and publishes its CA certificate to the MQTT end device.
7.	The MQTT end device further subscribes to __\<activation_code\>/wifi__ and publishes _GET_WIFI_CREDENTIALS_ request.
8.	In response, the MQTT broker issues WiFi credentials (SSID, Password) for the home network.
9.	When the MQTT end device receives the credentials, connection with the initialization broker is closed. The end device then connects to the home Wi-Fi with acquired credentials.
10.	Further MQTT end device connects to the MQTT broker with a certificate obtained in step 6 and subscribes to __\<device_id\>/topics__.
11.	In the next step, the MQTT end device publishes _GET_DEVICE_TOPICS_ request. The certificate from step 6 is directly connected to the __device id__. Thus only the device with proper __device id__ value can utilize this certificate. This approach brings additional security to the device configuration process.
12.	As a result, MQTT broker publishes the message to __\<home_id\>/\<gw_id\>/add_device__ topics with device type and __device id__ information.
13.	MQTT broker then expects a message from MQTT Controller with the end __device name__, __group__, and __id__.
14.	Based on the information from the previous step, the MQTT broker generates a topic structure for the end device and publishes the topics to the __\<dev_id\>/topics__.
15.	In this step, the MQTT end device subscribes to its topics, and all ongoing communication happens according to the MQTT2GO standard.



<p align="center" >
	<img src="mqtt_setup_cert.svg" alt="Proccess of adding a new MQTT2GO device">
</p>
<p align="center" >
	<a name="add-devices-fig"><em>Process of adding a new MQTT2GO device via guest WiFi.</em>
</p>

## Network Join
<p align="justify">
The operation of adding a new device into the network / household is utilizing a special topics. These topics are only temporary and utilized only for this specific use-case. This means that, aside from one, they are not a part of the standard topic structure. This simplifies the implementation at the end devices as they have no idea of the current structure of the MQTT2GO household. This topics and needed commands and reports are described in this section.
</p>

### Topics Structure
<p align="justify">
As aforementioned, network join topics are unique inside the MTT2GO standard and are solely used for the initial connection of new devices.
</p>

#### End device
<p align="justify">
The end device itself utilizes a unique topics for the initialization communication. They are:
</p>

```
<activation_code>/activation
```
utilized for certificate exchange,

```
<activation_code>/wifi
```
for the Wi-Fi credentials exchange.

### MQTT Commands
<p align="justify">
The MQTT commands for the network join are again based on the general structure from 2.1.2. And the most important change is in the value key of the command, which can be further divided by the function of the message (the numbering here corresponds to the one in [Process of adding a new MQTT2GO device via guest WiFi](#add-devices-fig)).
</p>

#### Get Credentials
<p align="justify">
This command (2) is utilized to get a newly generated certificate for the end device. Its command value is in form of string with value *GET_CREDENTIALS*.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "mqtt_credentials",
	"value": "GET_CREDENTIALS"
}
```

#### Get Wifi Credentials
<p align="justify">
Get Wifi credentials (4) which is used to obtain the Wi-Fi credentials, again the command value is <em>GET_WIFI_CREDENTIALS</em> string.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "wifi",
	"value": "GET_WIFI_CREDENTIALS"
}
```

### MQTT Reports
<p align="justify">
These reports are specifically designed for the initialization process of the network join. The again follow the general structure from 2.1.3. The are again labeled with numbers that are corresponding to the [Process of adding a new MQTT2GO device via guest WiFi](#add-devices-fig).
</p>

#### Credentials
This report (3) is utilized to deliver a newly generated certificate from MQTT broker to the end device.

```json
{
	"type": "report",
	"timestamp": "timestamp_value",
	"report_type": "mqtt_credentials",
	"value": "device_certificate"
}
```

#### Wi-Fi Credentials
<p align="justify">
This report (5) is used to send the Wi-Fi credentials back to the end device.
</p>

```json
{
	"type": "report",
	"timestamp": "timestamp_value",
	"report_type": "mqtt_credentials",
	"value": {
				"SSID": "wifi_ssid",
				"password": "password"
			 }
}
```

## Device Configuration
<p align="justify">
The device configuration is happening over topics that are unique to each device. This way it is secured that all the configuration will be done to the correct device. The only exception is the initialization process, where the topic is universal for the whole process, but in this case it is secured via the ability of adding only one device at a time.
</p>

## Topics Structure
<p align="justify">
The topics for the device configuration presented in this section are for the initial device configuration only. The device update and similar topics are presented in MQTT Objects section. Here, the topics are divided into two parts depending on which device is utilizing them.
</p>


#### Controlling App / SH-GW
<p align="justify">
The controlling App / SH-GW utilizes a special topic for the addition of new devices. This topic is structured as follows:
</p>

```
<home_id>/<gw_id>/add_device
```

<p align="justify">
Its main purpose is to publish information that initialize and finalize the adding process.
</p>

#### End Device
<p align="justify">
The end device utilizes this channel to get the list of the topics to which the devices has to be subscribed.
</p>

```
<dev_id>/topics
```

### MQTT Commands
<p align="justify">
The MQTT Commands mentioned below are used in adding a new device process. The command structure is based on the structure from 2.1.2. Again, the numbering in this section is coherent with the numbering in [Process of adding a new MQTT2GO device via guest WiFi](#add-devices-fig).
</p>

#### Activate Device
<p align="justify">
This command (1) is utilized to start the whole process of adding a new device. The command contains the activation code and id of the newly added device.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "activate_device",
	"value": {
				"activation_code": "activation_code",
				"device_id": "device_id"
			 }
}
```

#### Get Device Topics
<p align="justify">
Get device topics command (6) is used to get device topics from the SH-GW. This command has value of <em>GET_DEVICE_TOPICS</em>.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "topics",
	"value": "GET_DEVICE_TOPICS"
}
```

#### Rename Device
<p align="justify">
This command (8) is utilized to finalize the process of adding a new device to the system. Via this command, the end device gains its name and inclusion to the group.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "rename_device",
	"value": {
				"device_id": "device_id",
				"device_name": "device_name",
				"group_id": "group_id"
			 }
}
```

### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as a “responses” to aforementioned commands. Their structure is also coherent with the general structure from 2.1.3 and the numbering is matching the one in [Process of adding a new MQTT2GO device via guest WiFi](#add-devices-fig).
</p>


#### Rename Device
<p align="justify">
This report (7) is utilized to request the user of the MQTT2GO Controller app for the name and group of the newly added device.
</p>

```json
{
	"type": "report",
	"timestamp": "timestamp_value",
	"report_type": "rename_device",
	"value": {
				"device_type": "device_type",
				"device_id": "device_id"
			 }
}
```

#### Device Topics
<p align="justify">
This report is used to deliver the requested topics, in which the new device is intended to subscribe.
</p>

```json
{
	"type": "report",
	"timestamp": "timestamp_value",
	"report_type": "topics",
	"value": ["topic_1", "topic_2", "topic_3"]
}
```

[Back](./index.md#add-devices)
