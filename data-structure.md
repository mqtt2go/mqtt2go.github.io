
# MQTT2GO Data Structure
<p align="justify">
This section is dedicated to the MQTT2GO topic naming convention. The structure is organized into subsections based on the device types to provide an easier understanding of the division and key roles of each entity inside the MQTT2GO standard.
</p>

## General Commands and Reports
<p align="justify">
The general commands and reports are used to communicate device-independent messages. This means that all universal (common for all the devices) messages, such as errors and warnings are presented here. Furthermore, a general structure of command and report messages is given here. This structure is used for all MQTT2GO messages (both command and report ones).
</p>

### Topics Structure
<p align="justify">
The general MQTT2GO topic structure is created to be as efficient as possible, generating a reasonable amount of subtopics and therefore following the MQTT best practices. The topic itself is then composed as follows:
</p>

```
<home_id>/<gateway_id>/<group_id>/<device_type>/<dev_id>
```

<p align="justify">
where the <strong>&lt;home_id&gt;</strong> stands for the unique identificator of the home (this is used for the identification of a group of users, that are sharing one or more gateways and corresponding amount of devices connected to them),
<strong>&lt;gateway_id&gt;</strong> is the unique identificator of the gateway,
<strong>&lt;group_id&gt;</strong> is the unique identificator of the group of devices,
<strong>&lt;device_type&gt;</strong> defines to which category the device belongs,
and <strong>&lt;dev_id&gt;</strong> is device’s own unique identificator.
</p>

<p align="justify">
To access a multiple devices or a whole group. Wildcard masks from the MQTT standard needs to be used. If we want to substitute only one level, a <strong>+</strong> wildcard can be used. This means that the topic would look like:
</p>

```
<home_id>/<gateway_id>/+/<device_type>/<dev_id>
```

<p align="justify">
Which means that the subscribe/publish will be done to all groups, where the <strong>&lt;device_type&gt;/&lt;dev_id&gt;</strong> matches inserted data.
</p>

<p align="justify">
If the subscribe/publish should be to a larger group of end devices, a <strong>&#35;</strong> wildcard mask is used. This means that all topics after the <strong>&#35;</strong> are used:
</p>

```
<home_id>/<gateway_id>/#
```

<p align="justify">
Therefore means that the messages will go to all devices and all groups under selected gateway.
</p>


### MQTT Commands
<p align="justify">
The command messages are composed of four fields: (i) type, which is used to distinguish between the command and report type, (ii) timestamp, (iii) command type which is used to select the correct type of command (i.e., set, query, etc.) and (iv) command structure, containing the actual command. The command itself can be either a simple name-value pair or a complex structure, which is usually used for complex operations such as device setup.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "command_type_value",
	"value": "value_body"
}
```
<p align="justify">
The timestamp defines the datetime of the message sent event. It is in Unix format.
The command_type defines what information should be expected in the value key-pair. It can be any of the command types defined in the tables from sections 2.2.1.2, 2.2.2.2, 2.2.3.2, 2.3.2, 2.4.2. If the command_type_value will contain a set, a value of simple commands such as on can be expected. If command_type_value will contain a color keyword, the value will contain an array, which will describe the HSB information needed to set up the chosen color.<br>
Based on previous examples, the value key-pair can contain either a simple command such as on, off and similar, or more advanced commands represented by an array (i.e., the array for HSB information for setting the light color).
</p>

The general commands that are common for all devices are: 
* QueryOn, 
* QueryBattery, 
* QueryState, 
* QueryTamper, 
* QueryStatus.

### MQTT Commands
<p align="justify">
The report message structure is used for replies coming from the devices. The report messages can also contain a periodic update from the device. They are marked by the report type, which contains priority level to differentiate between critical reports (such as alarms), standard periodic messages and replies to commands.
</p>

```json
{
	"type": "report",
	"priority_level":"priority_level_value",
	"report_type":"report_type_name",
	"timestamp":"timestamp_value",
	"report_name":"report_name",
	"value":"value_body" 
}

```

<p align="justify">
The priority_level is used to set a message priority. It can be between 1-5, where 1 is the lowest and 5 the highest.
The report_type defines the type of report, there are four types of reports: (i) Status, (ii) CommandResponse, (iii) PeriodicReport,  and  (iv) Error.
The timestamp defines the datetime of the message sent event. It is in Unix format.
The <strong>report_name</strong> carries information about the value format. It can be any of the names from the afore presented tables. For example, when the <strong>report_name</strong> has a value of temperature, the value key-pair will carry an integer which corresponds to a degrees of celsius.
value_body can be either a simple response (OK), an integer, or array of values.
</p>

The general reports that are common for all devices are:
* OK,
* BatteryBad.

## System, Users, and Device Initialization/Management
<p align="justify">
This section describes the topic connected with the system, users, and device initialization and management. It is divided into three main parts: user management, network join, and device configuration.
</p>

### User Management
<p align="justify">
This section describes the topics that deal with user management. That being said, the core functionality of this module is to add, delete or update users inside a one MQTT2GO smart-home household (that does not necessarily mean one SH-GW). We distinguish between two user roles: (i) adult, who has the “administrator” access and therefore can control whole household without any restrictions, and (ii) children, which has a restricted access to selected features (i.e cannot arm/disarm the alarm or control any other security features).
</p>

#### Topics Structure
The base topic for user management is defined as: 

```
<home_id>/users
```

<p align="justify">
Inside this topic, the MQTT2GO commands are sent if any user-related operation is to be performed.
</p>

#### MQTT Commands
<p align="justify">
The MQTT commands for user management are based on the commands template from 2.1.2, where the MQTT <strong>command_type</strong> will be one of the following:
</p>

* add_user,
* updt_user
* del_user.

The JSON body for first two commands will be:

```json
{
	"user_id": "id",
	"email": "email",
	"user_name": "name",
	"role": "role"
}
```

and for the deletion operation:

```json
{
	"user_id": "id"
}
```

The concrete value of each parameter depends on the specific use-case.

#### MQTT Reports
<p align="justify">
The MQTT reports for the user management are based on the commands template from 2.1.3, where the MQTT report_type corresponds to the commands from: MQTT Commands, with the JSON body containing the operation results:
</p>

```json
{
	"result": "result"
}
```

The result itself can be:
* OK,
* Error.

#### Examples
The example of a user add operation can look like:

```json
{
	"user_id": 123456789,
	"email": "john.doe@mail.com",
	"user_name": "John Doe",
	"role": "adult"
}
```

and the response will be:

```json
{
	"result": "ok" 
}
```

### Network Join
<p align="justify">
The operation of adding a new device into the network / household is utilizing a special topics. These topics are only temporary and utilized only for this specific use-case. This means that, aside from one, they are not a part of the standard topic structure. This simplifies the implementation at the end devices as they have no idea of the current structure of the MQTT2GO household. This topics and needed commands and reports are described in this section.
</p>

#### Topics Structure
<p align="justify">
As aforementioned, network join topics are unique inside the MTT2GO standard and are solely used for the initial connection of new devices. The topics are further divided into two groups based on who is communicating:
</p>

*Controlling App / SH-GW*
<p align="justify">
The controlling App / SH-GW utilizes a special topic for the addition of new devices. This topic is structured as follows:
</p>

```
<home_id>/<gw_id>/add_device
```

<p align="justify">
Its main purpose is to publish information that initialize and finalize the adding process.
</p>


*End device*
<p align="justify">
The end device itself utilizes a unique topics for the initialization communication. They are:
</p>

* __\<dev_id\>/activation__ for the initial key exchange,
* __\<dev_id\>/wifi__ for the Wi-Fi credentials exchange,
* __\<dev_id\>/credentials__ for the MQTT login information exchange, and
* __\<dev_id\>/topics used__ to get the list of the topics to which the devices has to be subscribed.

#### MQTT Commands
<p align="justify">
The MQTT commands for the network join are again based on the general structure from 2.1.2. And the most important change is in the value key of the command, which can be further divided by the function of the message (the numbering here corresponds to the one in Fig.
</p>

##### Activation Code
<p align="justify">
Activation code which is used for the whole add a new device process initialization. This message is the only one from the network join section, which is sent to the <strong>&lt;home_id&gt;/&lt;gw_id&gt;/add_device</strong> topic. Its value is set to the activation code, which is a unique identifier provided by a manufacturer on the being added device.
</p>

##### Get encryption keys
Get encryption keys is used to obtain the encryption key, this commands value is simple GET_ENCRYPTION_KEYS string.

##### Get Wifi Credentials
Get Wifi credentials which is used to obtain the Wi-Fi credentials, again the command value is GET_WIFI_CREDENTIALS string.

##### Get MQTT credentials
Get MQTT credentials, utilized to request the MQTT credentials by sending a command with value o GET_MQTT_CREDENTIALS.

#### MQTT Reports
<p align="justify">
These reports are specifically designed for the initialization process of the network join. The again follow the general structure from 2.1.3. The are again labeled with numbers that are corresponding with the Fig.
</p>

##### Key A, g, p
<p align="justify">
Key A, g, p this message is used to exchange the key between the device and SH-GW. Its value is a JSON with following structure.
</p>

```json
{
	"dev_id": "device_id",
	"Key_g": "g_value",
	"Key_p": "p_value",
	"key_a" : "a_value"
}
```

##### Key B
Key B, which has the same structure as previous, but there is only.

```json
{
	"key_b" : "b_value"
}
```

##### Wi-Fi credentials
<p align="justify">
Wi-Fi credentials, this message is used to send the Wi-Fi credentials back to the end device. The values of this message are encrypted via AES. Its value is again a JSON with following structure.
</p>

```json
{
	"SSID": "wifi_ssid",
	"password": "password"
}
```

##### MQTT credentials
<p align="justify">
MQTT credentials which is also AES encrypted and used to obtain the correct MQTT client credentials.
</p>

```json
{
	"login": "device_id",
	"password": "password"
}
```

### Device Configuration
<p align="justify">
The device configuration is happening over topics that are unique to each device. This way it is secured that all the configuration will be done to the correct device. The only exception is the initialization process, where the topic is universal for the whole process, but in this case it is secured via the ability of adding only one device at a time. 
</p>

#### Topics Structure
<p align="justify">
The topics for the device configuration presented in this section are for the initial device configuration only. The device update and similar topics are presented in MQTT Objects section. Here, the topics are divided into two parts depending on which device is utilizing them:
</p>

1. __/user_id/gw_id/add_device__ utilized by the controlling apps,
2. __/dev_id/topics reserved for the end devices__.

#### MQTT Commands
<p align="justify">
The MQTT Commands mentioned below are used in adding a new device process. The command structure is based on the structure from 2.1.2.  Again, the numbering in this section is coherent with the numbering in Fig. ADD FIG REF.
</p>

##### Get device topics
<p align="justify">
Get device topics command is used to get device topics from the SH-GW. This command has value of GET_DEVICE_TOPICS.
</p>


##### Get Device group and name
<p align="justify">
Get Device group and name is used to request the configuration information from the controlling app. Its value is JSON body with following structure.
</p>

```json
{
	"device_type": "device_type",
	"device_id":"device_id"
}
```

#### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as a “responses” to aforementioned commands. Their structure is also coherent with the general structure from 2.1.3 and the numbering is matching the one in Fig: ADD FIG REF.
</p>

##### Successfully added device
<p align="justify">
Successfully added device is topic utilized for the confirmation from the controlling app with the final device name and group. It value is JSON body of following structure.
</p>

```json
{
	"device_id":"device_id",
	"device_name": "device_name",
	"device_group": "device_group"
}
```

##### Device topics
To Do

## MQTT Objects
<p align="justify">
MQTT objects are describing all end devices that are connected to the MQTT2GO system. Their primary purpose is to either periodically report measurement results back to the SH-GW or to fulfill their role in the smart home.
</p>

### Topics Structure
<p align="justify">
The topic structure for MQTT objects is in line with the general structure described in 2.1.1. The topic structure is following.
</p>

```
<home_id>/<gateway_id>/<group_id>/<device_type>/<dev_id>
```

### MQTT Commands
<p align="justify">
The MQTT Commands described in this section are device specific and therefore they are arranged in tables for better understanding. The structure of the command is again compliant to the general structure from  2.1.2.
</p>

#### Description of command types
<p align="justify">
The command types used in the MQTT Commands are describing the targeted functionality of the whole command. The command types are:
</p>

* Set commands, which can be further specified by underscore to match a certain command. I.e., set_temperature.
* Query commands, which are used to query information from the end devices. They are used to force the readout out of the periodic report time cycle.

#### Table with commands

| Name | Device type | Command Type | Command |
| --- | --- | --- | --- |
|Garage door|garage_door|set<br/>set_timer|Up, Down, Stop<br/>&lt;Timer&gt;|


[Back](./)
