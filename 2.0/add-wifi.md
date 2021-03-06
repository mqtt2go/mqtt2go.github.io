[Back](./index.md#add-devices)
# Setup via Guest Wi-Fi
<p align="justify" >
The following process of adding new device should be considered as the optimal way, how to add new device / sensors into the MQTT2GO Smart Home ecosystem. This way, we can provide the highest security to the users, as the Wi-Fi utilized for adding new devices is active only for the short period of time during the first step. The ideal process to add a new device is based on the following steps.
</p>

## Setup Steps

1.	MQTT Controller (Mobile/Web App) initiates the process of adding a new device by subscribing to __/\<home_id\>/\<gw_id\>/add_device__. Then it publishes an activation request containing __activation code, device id__, and __device type__. The __activation code__ and __device id__ codes can be found on a newly installed device as two unique numbers identifying the device (placed on a device label or inside a QR code).
2.	In response to the request, SH-GW enables the guest Wi-Fi with SSID of __MQTT2GO_Config__, the activation code serves as password.
3.	MQTT end device (ED) then connects to the guest Wi-Fi and further utilizes mDNS (multicast DNS) to resolve address __MQTT2GO.local__ (\_mqtt.\_tcp.local.) and port, which equals to the address and port, respectively, of the local MQTT broker.
4.	The MQTT ED then connects to the local MQTT broker under the initialization account (only a pre-loaded certificate is used for login). The MQTT ED contains a pre-loaded certificate fingerprint of trustworthy CA (Certification Authority), which is used to establish TLS (Transport Layer Security) communication with the MQTT broker. The chain of trust must be on both sides. Thus, the SH-GW (MQTT broker) has to contain a certificate issued by the same CA as it is contained in the MQTT ED. If these certificates do not match, the TLS communication cannot be established, and the device will be refused as a non-trustworthy.
5.	When the TLS communication is established, the MQTT ED subscribes to the __\<activation_code\>/activation/in__ topic and publishes _GET_CREDENTIALS_ request to the __\<activation_code\>/activation/out__ topic.
6.	As a response, SH-GW (local MQTT broker) generates a new set of certificates that will be used for ongoing communication and publishes its CA certificate, login, and password to the MQTT ED's __\<activation_code\>/activation/in__ topic.
7.	The MQTT ED further subscribes to __\<activation_code\>/wifi/in__ and publishes _GET_WIFI_CREDENTIALS_ request to __\<activation_code\>/wifi/out__.
8.	In response, the local MQTT broker issues Wi-Fi credentials (SSID, Password) for the home network. The Wi-Fi password is securely stored in SH-GW, providing the credentials to the local MQTT broker.
9.	When the MQTT ED receives the credentials, connection under the initialization account to the local broker is closed. The ED then connects to the home Wi-Fi with the acquired credentials.
10.	Further MQTT ED connects to the local MQTT broker with a certificate and credentials obtained in the step 6 and subscribes to __\<dev_id\>/home/in__. The certificate from the step 6 is directly connected to the __device id__. Thus only the device with proper __device id__ value can utilize this certificate. This approach brings additional security to the device configuration process.
11.	In the next step, the MQTT ED publishes request of the _home_prefix_ type to __\<dev_id\>/home/in__ topic that contains all entities the ED provides with additional information on its data type and unit. 
12. The SH-GW publishes _home_id_ and _gw_id_ to __\<dev_id\>/home/in__. The MQTT2GO ED uses these prefixes to generate unique topics for each of its entities. As the local MQTT broker possesses information about all ED entities, it can generate topics on the local broker side.
13.	As a result, MQTT broker publishes the message to __\<home_id\>/\<gw_id\>/add_device/in__ topic with __device id__ information to distinguish individual devices. This message indicates to the controller that new ED was added to the system, and it is necessary to set a human-readable name and assign ED to the group(s).
14.	The local MQTT broker then expects a message from MQTT Controller with the end device __name__, __group__, and __device_id__.
15.	In what follows, the MQTT end device subscribes to its topic, and all ongoing communication happens according to the MQTT2GO standard.


<p align="center" >
	<img src="mqtt_setup_cert.svg" alt="Proccess of adding a new MQTT2GO device">
</p>
<p align="center" >
	<a name="add-devices-fig"></a><em><strong>Fig. 1:</strong> Process of adding a new MQTT2GO device via guest Wi-Fi.</em>
</p>

## Network Join
<p align="justify">
The operation of adding a new device into the network / household is exploiting a special topics. These topics are only temporary and utilized only for this specific use-case.
</p>

### Topics Structure
<p align="justify">
As aforementioned, network join topics are unique inside the MTT2GO standard and are solely used for the initial connection of new devices. Their structure is given in following subsections.
</p>

### MQTT Commands
<p align="justify">
The MQTT commands for the network join are again based on the general structure from <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>. The most important difference is the <i>value</i> of the command, which can be further divided by the function of the command message (the numbering here corresponds to the one in <a href="#add-devices-fig">Fig. 1</a>).
</p>

#### Get Credentials
<p align="justify">
This command (2) is utilized to get a newly generated certificate for the end device. Its command <i>value</i> is in form of string with value <em>GET_CREDENTIALS</em>.
</p>

```
<activation_code>/activation/out
```
```json
{
    "timestamp": "timestamp_value",
    "type": "mqtt_credentials",
    "value": "GET_CREDENTIALS"
}
```

#### Get Wifi Credentials
<p align="justify">
In case of Get Wi-Fi credentials (4), which is used to obtain the Wi-Fi credentials, again the command <i>value</i> is <em>GET_WIFI_CREDENTIALS</em> string.
</p>

```
<activation_code>/wifi/in
```
```json
{
    "timestamp": "timestamp_value",
    "type": "wifi_credentials",
    "value": "GET_WIFI_CREDENTIALS"
}
```

### MQTT Reports
<p align="justify">
These reports are specifically designed only for the network join. They follow the general structure from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a>, and they are labeled with numbers that correspond to the <a href="#add-devices-fig">Fig. 1</a>.
</p>

#### Credentials Report
This report (3) is utilized to deliver a newly generated certificate, password, and login from MQTT broker to the end device.

```
<activation_code>/activation/in
```
```json
{
    "timestamp": "timestamp_value",
    "type": "mqtt_credentials",
    "value":  {
        "cert": "device_certificate",
        "user": "mqtt_login",
        "password": "mqtt_password"
    }
}
```

#### Wi-Fi Credentials report
<p align="justify">
This report (5) is used to send the Wi-Fi credentials back to the end device. As the TLS connection is active initially, the credentials are transmitted in the secure (encrypted) way.
</p>

```
<activation_code>/wifi/in
```
```json
{
    "timestamp": "timestamp_value",
    "type": "wifi_credentials",
    "value": {
        "SSID": "wifi_ssid",
        "password": "password"
    }
}
```

## Device Configuration
<p align="justify">
The device configuration is happening over topics that are unique to this only process. To secure that the configuration will be done to the intended device, a unique <strong>device_ids</strong> is utilized during the process. 
</p>

### Topics Structure
<p align="justify">
The topics for the device configuration presented in this section are for the initial device configuration only.
</p>

### MQTT Commands
<p align="justify">
The MQTT Commands mentioned below are used in adding a new device process. The command structure is based on the structure from <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>. Again, the numbering in this section is coherent with the numbering in <a href="#add-devices-fig">Fig. 1</a>.
</p>

#### Add New Device
<p align="justify">
This command (1) is utilized to start the whole process of adding a new device. The command contains the <strong>activation_code</strong>, <strong>device_id</strong>, and <strong>device_type</strong> of the newly added device.
</p>

```
<home_id>/<gw_id>/add_device/out
```

```json
{
	"timestamp": "timestamp_value",
	"value": {
		"activation_code": "activation_code",
		"device_id": "device_id",
		"device_type": "device_type"
	}
}
```

#### Get Home Prefix
<p align="justify">
In case of Get Home Prefix (6), which is used to obtain the home prefix, that consists of <em>home_id</em> and <em>gateway_id</em>. The command <i>value</i> consists of array of the available topic names of the current device, together with unit name and type.
</p>

```
<device_id>/home/out
```

```json
{
    "timestamp": "timestamp_value",
    "type": "home_prefix",
    "value": [
      {"name": "topic_name",
       "unit": "unit_quantity",
       "type": "unit_datatype"
      }, ...   
    ]
}
```

#### Rename Device
<p align="justify">
This command (9) is utilized to finalize the process of adding a new device to the system. Via this report, the end device gains its name and inclusion to the groups.
</p>

```
<home_id>/<gw_id>/add_device/out
```

```json
{
	"timestamp": "timestamp_value",
	"value": {
		"device_id": "device_id",
		"device_name": "device_name",
		"group_id": ["group_id_1", "group_id_2", ...]
	}
}
```

### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as “responses” to aforementioned commands. Their structure is also coherent with the general structure from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a> and the numbering is matching the one in <a href="#add-devices-fig">Fig. 1</a>.
</p>

#### Home Prefix Report
<p align="justify">
The home prefix report (7) consists of <em>home_id</em> and <em>gateway_id</em>.
</p>

```
<device_id>/home/in
```

```json
{
    "timestamp": "timestamp_value",
    "type": "home_prefix",
    "value": {
      "home_id": "home_id_value",
       "gateway_id": "gateway_id_value"
    }
}
```

#### Rename Device Report
<p align="justify">
This report (8) is utilized to request the user of the <a href="./mqtt2go-controllers">MQTT2GO Controller</a> app for the name and group of the newly added device.
</p>


```
<home_id>/<gw_id>/add_device/in
```

```json
{
	"timestamp": "timestamp_value",
	"value": {
		"device_id": "device_id",
		"setup_result": "setup_result"
	}
}
```

[Back](./index.md#add-devices)
