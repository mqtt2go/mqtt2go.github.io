[Back](./index.md#data-structure)
# System, Users, and Device Initialization/Management
<p align="justify">
This section describes the topic connected with the system, users, and device initialization and management. It is divided into three main parts: user management, network join, and device configuration.
</p>

## User Management
<p align="justify">
This section describes the topics that deal with user management. That being said, the core functionality of this module is to add, delete or update users inside a one MQTT2GO smart-home (which does not necessarily mean one SH-GW). We distinguish between three user roles: (i) Administrator, who has full access to all features and therefore can control whole household without any restrictions, (ii) family member, who can control the whole smart-home but cannot manage users, and guest, who can control only utility home devices, such as lights nad smart TVs.
</p>

[logo]: https://github.githubassets.com/images/icons/emoji/unicode/274c.png?v8&s=10 "Cross"

| User Role                   | Administrator | Family member | Guest |
|-----------------------------|:---------------:|:---------------:|:-------:|
| User management             | ✔               | 🗙              | 🗙      |
| Security device control     | ✔ | ✔ | 🗙 |
| Utility home device control | ✔ | ✔ | ✔  |

### Topics Structure
The base topic for user management is defined as: 

```
<home_id>/users
```

<p align="justify">
Inside this topic, the MQTT2GO commands are sent if any user-related operation is to be performed.
</p>

### MQTT Commands
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

### MQTT Reports
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

### Examples
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

## Network Join
<p align="justify">
The operation of adding a new device into the network / household is utilizing a special topics. These topics are only temporary and utilized only for this specific use-case. This means that, aside from one, they are not a part of the standard topic structure. This simplifies the implementation at the end devices as they have no idea of the current structure of the MQTT2GO household. This topics and needed commands and reports are described in this section.
</p>

### Topics Structure
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

### MQTT Commands
<p align="justify">
The MQTT commands for the network join are again based on the general structure from 2.1.2. And the most important change is in the value key of the command, which can be further divided by the function of the message (the numbering here corresponds to the one in Fig.
</p>

#### Activation Code
<p align="justify">
Activation code which is used for the whole add a new device process initialization. This message is the only one from the network join section, which is sent to the <strong>&lt;home_id&gt;/&lt;gw_id&gt;/add_device</strong> topic. Its value is set to the activation code, which is a unique identifier provided by a manufacturer on the being added device.
</p>

#### Get encryption keys
Get encryption keys is used to obtain the encryption key, this commands value is simple GET_ENCRYPTION_KEYS string.

#### Get Wifi Credentials
Get Wifi credentials which is used to obtain the Wi-Fi credentials, again the command value is GET_WIFI_CREDENTIALS string.

#### Get MQTT credentials
Get MQTT credentials, utilized to request the MQTT credentials by sending a command with value o GET_MQTT_CREDENTIALS.

### MQTT Reports
<p align="justify">
These reports are specifically designed for the initialization process of the network join. The again follow the general structure from 2.1.3. The are again labeled with numbers that are corresponding with the Fig.
</p>

#### Key A, g, p
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

#### Key B
Key B, which has the same structure as previous, but there is only.

```json
{
	"key_b" : "b_value"
}
```

#### Wi-Fi credentials
<p align="justify">
Wi-Fi credentials, this message is used to send the Wi-Fi credentials back to the end device. The values of this message are encrypted via AES. Its value is again a JSON with following structure.
</p>

```json
{
	"SSID": "wifi_ssid",
	"password": "password"
}
```

#### MQTT credentials
<p align="justify">
MQTT credentials which is also AES encrypted and used to obtain the correct MQTT client credentials.
</p>

```json
{
	"login": "device_id",
	"password": "password"
}
```

## Device Configuration
<p align="justify">
The device configuration is happening over topics that are unique to each device. This way it is secured that all the configuration will be done to the correct device. The only exception is the initialization process, where the topic is universal for the whole process, but in this case it is secured via the ability of adding only one device at a time. 
</p>

### Topics Structure
<p align="justify">
The topics for the device configuration presented in this section are for the initial device configuration only. The device update and similar topics are presented in MQTT Objects section. Here, the topics are divided into two parts depending on which device is utilizing them:
</p>

1. __/user_id/gw_id/add_device__ utilized by the controlling apps,
2. __/dev_id/topics__ reserved for the end devices.

### MQTT Commands
<p align="justify">
The MQTT Commands mentioned below are used in adding a new device process. The command structure is based on the structure from 2.1.2.  Again, the numbering in this section is coherent with the numbering in Fig. ADD FIG REF.
</p>

#### Get device topics
<p align="justify">
Get device topics command is used to get device topics from the SH-GW. This command has value of GET_DEVICE_TOPICS.
</p>


#### Get Device group and name
<p align="justify">
Get Device group and name is used to request the configuration information from the controlling app. Its value is JSON body with following structure.
</p>

```json
{
	"device_type": "device_type",
	"device_id":"device_id"
}
```

### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as a “responses” to aforementioned commands. Their structure is also coherent with the general structure from 2.1.3 and the numbering is matching the one in Fig: ADD FIG REF.
</p>

#### Successfully added device
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

#### Device topics
To Do

[Back](./index.md#data-structure)
