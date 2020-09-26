[Back](./index.md#add-devices)
# Setup via WPS
<p align="justify">
The process of adding a new device using the WPS (WiFi Protected Setup) is very similar to the setup via Wi-Fi. The only difference is that all the initial setup of the connection to the SH-GW is done via the WPS. The process of WPS is as follows: both SH-GW and new ED (end device) have to activate the WPS at the same time. After the WPS is activated, the client-side device negotiates access with the access point and after the Wi-Fi connection is set up, the rest is the same as in the setup via Wi-Fi example (as described in <a href="./add-wifi">Setup via Guest WiFi</a>).
</p>

## Setup Steps
1.	MQTT Controller (Mobile/Web App) initiates the process of adding a new device by subscribing to __/\<home_id\>/\<gw_id\>/add_device/in__. Then it publishes an activation request containing __activation code, device id__, and __device type__ to __/\<home_id\>/\<gw_id\>/add_device/out__. The __activation code__ and __device id__ codes can be found on the newly installed device in the form of two unique numbers identifying the device or as QR code.
2.	The user is prompted to activate WPS by pressing buttons SH-GW.
3.	MQTT ED then connects to the Home Wi-Fi and further utilizes mDNS (multicast DNS) to resolve address __MQTT2GO.local__ (\_mqtt.\_tcp.local.) and port, which equal to the address and port of local MQTT broker, respectively.
4.	The MQTT ED then connects to the initialization MQTT broker. The MQTT ED contains a pre-loaded certificate of trustworthy CA (Certification Authority), which is used to establish TLS (Transport Layer Security) communication with the MQTT broker. The chain of trust must be on both sides. Thus, the SH-GW (MQTT broker) has to contain a certificate issued by the same CA as it is contained in the MQTT ED. If these certificates do not match, the TLS communication cannot be established.
5.	When the TLS communication is established the MQTT ED subscribes to the __\<activation_code\>/activation/in__ topic and publishes _GET_CREDENTIALS_ request to __\<activation_code\>/activation/out__.
6.	As a response, SH-GW (MQTT broker) generates a new set of certificates that will be used for ongoing communication and publishes its CA certificate, login, and password to the MQTT ED.
7.	When the MQTT ED receives the certificate and MQTT credentials from the __\<activation_code\>/activation/in__ topic, a connection with the initialization broker is closed.
8.	Further MQTT ED connects to the MQTT broker with a certificate and credentials obtained in the step 6 and subscribes to __\<dev_id\>/home/in__. The certificate from the step 6 is directly connected to the __device id__. Thus only the device with proper __device id__ value can utilize this certificate. This approach brings additional security to the device configuration process.
9.	In the next step, the MQTT ED publishes request of the _home_prefix_ type to __\<dev_id\>/home/in__ topic that contains all entities the ED provides with additional information on its data type and unit. 
10. The SH-GW publishes _home_id_ and _gw_id_ to __\<dev_id\>/home/in__.
11.	As a result, MQTT broker publishes the message to __\<home_id\>/\<gw_id\>/add_device/in__ topic with __device id__ information to distinguish individual devices.
12.	MQTT broker then expects a message from MQTT Controller with the end device __name__, __group__, and __device_id__.
13.	In what follows, the MQTT end device subscribes to its topic, and all ongoing communication happens according to the MQTT2GO standard.


<p align="center" >
	<img src="mqtt_wps_setup_cert.svg" alt="Process of adding new MQTT2GO device utilizing WPS">
</p>
<p align="center" >
	<a name="add-devices-fig"></a><em><strong>Fig. 1: </strong>Process of adding new MQTT2GO device utilizing WPS.</em>
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
In case of Get Home Prefix (4), which is used to obtain the home prefix, that consists of <em>home_id</em> and <em>gateway_id</em>. The command <i>value</i> consists of array of the available topic names of the current device, together with unit name and type.
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
This report (6) is utilized to request the user of the <a href="./mqtt2go-controllers">MQTT2GO Controller</a> app for the name and group of the newly added device.
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

### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as “responses” to aforementioned commands. Their structure is also coherent with the general structure from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a> and the numbering is matching the one in <a href="#add-devices-fig">Fig. 1</a>.
</p>

#### Home Prefix Report
<p align="justify">
The home prefix report (5) consists of <em>home_id</em> and <em>gateway_id</em>.
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
This report (7) is utilized to finalize the process of adding a new device to the system. Via this report, the end device gains its name and inclusion to the groups.
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

[Back](./index.md#add-devices)
