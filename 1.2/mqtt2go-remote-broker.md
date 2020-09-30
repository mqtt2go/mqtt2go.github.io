[Back](./index.md#add-devices)
# Setup via WPS with Remote Broker
<p align="justify">
On top of the typical Smart-Home paradigm with a centerpiece of all communication in the form of SH-GW, MQTT2GO also offers to offload this functionality to the cloud. The local network then does not contains any local MQTT broker, but the whole communication is handled in the cloud. Nevertheless, none of the features of the local broker is missing, and the transition is seamless from the perspective of the end-user.	
</p>

<p align="justify">
From the perspective of the whole communication infrastructure, one new element is required to be presented. This element is called the Discovery server and allows end devices to discover the address of the cloud MQTT broker. Further, an additional communication channel between the cloud broker and the Discovery server is needed, as it will be described in the following text.
</p>

<p align="justify">
The following steps cover the optimal case with WPS functionality. When the WPS is not present, the process is more complicated and requires additional user interaction. These steps mainly consist of delivering the SSID and password of home Wi-Fi.
</p>

## Setup Steps
1.	MQTT Controller (Mobile/Web App) initiates the process of adding a new device by subscribing to __/\<home_id\>/\<gw_id\>/add_device__. Then it publishes an activation request containing __activation code__ and __device id__. These codes can be found on the newly installed device in the form number two numbers or QR code.
2.	The MQTT broker then contacts the Discovery server and adds the newly installed device into the database of devices waiting for activation. The communication is conducted via __REST API__ over HTTPS protocol. All data including cloud MQTT __broker IP__, __activation code__ and newly generated __certificate__, __login__, and __password__ are delivered to the __/activate_device__ endpoint. The device is not stored in the database permanently, but it is removed after 15 minutes. This approach limits the time window during which the device can be activated, but also adds an additional level of security.
3.	In the meantime, the MQTT end device connects to the home Wi-Fi via WPS and looks for the network for the __MQTT2GO.local__ (\_mqtt.\_tcp.local.) address.
4.	When the MQTT broker is not founded in the local network, end device contacts the Discovery server. The address of the server is pre-loaded in the device during the manufacturing process.
5.	The end device utilizes the pre-loaded certificate together with the activation code to connect to the MQTT broker on the Discovery server. As in the case of other activation procedures, the certificate of the server must be issued by the same CA (Certification Authority). Otherwise, the TLS (Transport Layer Security) communication cannot be established.
6.	Further, the end device subscribes to the __\<activation_code\>/activation__ topic and publishes the _GET_BROKER_DATA_ request.
7.	As a response MQTT broker on the Discovery server delivers the cloud __broker IP__, __login__, __password__, and the new __certificate__ obtained in step 5. The end device further closes the connection to the MQTT broker on the Discovery server.
8.	Then the end device connects to the remote MQTT broker with the certificate and credentials from the previous step and subscribes to __\<dev_id\>/topic__.
9.	In the next step, the MQTT end device publishes _GET_DEVICE_TOPIC_ request.
10.	As a result, the MQTT broker publishes the message to __\<home_id\>/\<gw_id\>/add_device__ topic with device type and __device id__ information.
11.	MQTT broker then expects a message from MQTT Controller with the end device __name__, __group__, and __id__.
12.	Based on the information from the previous step, the MQTT broker generates a topic structure for the end device and publishes the device topic to the __\<dev_id\>/topic__.
13.	In this step, the MQTT end device subscribes to its topic, and all ongoing communication happens according to the MQTT2GO standard.


<p align="center" >
	<img src="mqtt_remote_broker.svg" alt="Proccess of adding a new MQTT2GO device">
</p>
<p align="center" >
	<a name="add-devices-fig"></a><em><strong>Fig. 1:</strong> Process of adding a new MQTT2GO device with remote broker.</em>
</p>

## Network Join
<p align="justify">
In this phase, no MQTT communication took place, and the whole process consist of network join via the WPS process.
</p>

## MQTT Broker Discovery
<p align="justify">
The operation of adding a new device into the network / household is utilizing a special topics. These topics are only temporary and utilized only for this specific use-case. This means that, aside from one, they are not a part of the standard topic structure. This simplifies the implementation at the end devices as they have no idea of the current structure of the MQTT2GO household. The topics, commands and reports needed for discovery of remote MQTT broker are described in this section.
</p>

### Topics Structure
<p align="justify">
As aforementioned, network join topics are unique inside the MTT2GO standard and are solely used for the initial connection of new devices.
</p>

#### End device
<p align="justify">
The end device itself utilizes an activation topic for certificate exchange.
</p>

```
<activation_code>/activation
```

### MQTT Commands
<p align="justify">
The MQTT commands for the network join are again based on the general structure from <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>. And the most important change is in the value key of the command, which can be further divided by the function of the message (the numbering here corresponds to the one in <a href="#add-devices-fig">Fig. 1</a>).
</p>

#### Get Remote Broker Data
<p align="justify">
This command (3) is utilized to get the address of remote broker together with a newly generated certificate for the end device. Its command value is in form of string with value <em>GET_BROKER_DATA</em>.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "mqtt_credentials",
	"value": "GET_BROKER_DATA"
}
```

### MQTT Reports
<p align="justify">
These reports are specifically designed for the initialization process of remote broker discovery. Again the process follows the general structure from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a>. They are again labeled with numbers that are corresponding to the <a href="#add-devices-fig">Fig. 1</a>.
</p>

#### Remote Broker Data
This report (4) is utilized to deliver a newly generated certificate from MQTT broker to the end device.

```json
{
	"type": "report",
	"report_type":"command_response",
	"timestamp": "timestamp_value",
	"report_name": "mqtt_credentials",
	"value": {
		"broker_ip": "broker_ip",
		"device_certificate": "device_certificate",
		"user": "mqtt_login",
		"password": "mqtt_password"
	}
}
```


## Device Configuration
<p align="justify">
The device configuration is happening over topics that are unique to each device. This way it is secured that all the configuration will be done to the correct device. The only exception is the initialization process, where the topic is universal for the whole process, but in this case it is secured via the ability of adding only one device at a time.
</p>

## Topics Structure
<p align="justify">
The topics for the device configuration presented in this section are for the initial device configuration only. The device update and similar topics are presented in <a href="./mqtt2go-objects">MQTT Objects</a> section. Here, the topics are divided into two parts depending on which device is utilizing them.
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
The end device utilizes this channel to get the topic to which the devices has to be subscribed.
</p>

```
<dev_id>/topic
```

### MQTT Commands
<p align="justify">
The MQTT Commands mentioned below are used in adding a new device process. The command structure is based on the structure from <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>. Again, the numbering in this section is coherent with the numbering in <a href="#add-devices-fig">Fig. 1</a>.
</p>

#### Activate Device
<p align="justify">
This command (1) is utilized to start the whole process of adding a new device. The command contains the <strong>activation_code</strong>, <strong>device_id</strong>, and <strong>device_type</strong> of the newly added device.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "activate_device",
	"value": {
		"activation_code": "activation_code",
		"device_id": "device_id",
		"device_type": "device_type"
	}
}
```

#### Get Device Topic
<p align="justify">
Get device topic command (5) is used to get device topic from the SH-GW. This command has value of <em>GET_DEVICE_TOPIC</em>.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "topic",
	"value": "GET_DEVICE_TOPIC"
}
```

#### Rename Device
<p align="justify">
This command (7) is utilized to finalize the process of adding a new device to the system. Via this command, the end device gains its name and inclusion to the groups.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "rename_device",
	"value": {
		"device_id": "device_id",
		"device_name": "device_name",
		"group_id": ["group_id_1", "group_id_2", ...]
	}
}
```

### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as a “responses” to aforementioned commands. Their structure is also coherent with the general structure from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a> and the numbering is matching the one in <a href="#add-devices-fig">Fig. 1</a>.
</p>


#### Rename Device
<p align="justify">
This report (6) is utilized to request the user of the MQTT2GO Controller app for the name and group of the newly added device.
</p>

```json
{
	"type": "report",
	"report_type":"command_response",
	"timestamp": "timestamp_value",
	"report_name": "rename_device",
	"value": {
		"device_id": "device_id",
		"setup_result": "setup_result"
	}
}
```

#### Device Topic
<p align="justify">
This report (8) is used to deliver the requested topic, in which the new device is intended to subscribe.
</p>

```json
{
	"type": "report",
	"report_type":"command_response",
	"timestamp": "timestamp_value",
	"report_name": "topic",
	"value": "topic"
}
```

[Back](./index.md#add-devices)