[Back](./index.md#add-devices)
# Setup via Guest WiFi
The ideal process of adding new device should be considered as the right way how to go through this process. Keeping this in mind, some of the steps detailed below can be reduced using a specific technology that can provide the needed functionality (i.e., WPS). The idealprocess to add a new device is based on the following steps.

## Setup Steps

1.	MQTT Controller (Mobile/Web App) initiates the process of adding a new device by subscribing to __/\<home_id\>/\<gw_id\>/add_device__. Then it publishes an activation request containing activation code and device id. These codes can be found on the newly installed device in the form number or QR code.
2.	In response to the request, SH-GW enables the Guest Wi-Fi and sets the password inputted as an activation code.
3.	MQTT end device then connects to the Guest Wi-Fi and further utilizes mDNS (multicast DNS) to resolve address __MQTT2GO.local__, which equals to the address of the MQTT broker.
4.	The MQTT end device then connects to the initialization MQTT broker. The MQTT end device contains a pre-loaded certificate of trustworthy CA (Certification Authority), which is used to establish TLS (Transport Layer Security) communication with the MQTT broker. The chain of trust must be on both sides. Thus, the SH-GW (MQTT broker) has to contain a certificate issued by the same CA as it is contained in the MQTT end-device. If these certificates do not match, the TLS communication cannot be established.
5.	When the TLS communication is established the MQTT end device subscribes to the __\<activation_code\>/activation__ topic and publishes _GET_CREDENTIALS_ request.
6.	As a response, SH-GW (MQTT broker) generates a new set of certificates that will be used for ongoing communication and publishes its CA certificate to the MQTT end device.
7.	The MQTT end device further subscribes to __\<activation_code\>/wifi__ and publishes _GET_WIFI_CREDENTIALS_ request.
8.	In response, the MQTT broker issues WiFi credentials (SSID, Password) for the home network.
9.	When the MQTT end device receives the credentials, connection with the initialization broker is closed. The end device then connects to the home Wi-Fi with acquired credentials.
10.	Further MQTT end device connects to the MQTT broker with a certificate obtained in step 6 and subscribes to __\<device_id\>/topics__.
11.	In the next step, the MQTT end device publishes _GET_DEVICE_TOPICS_ request.
12.	As a result, MQTT broker publishes message to __\<home_id\>/\<gw_id\>/add_device__ topics with device type and device id information.
13.	MQTT broker then expects message from MQTT Controller with the end device name, group, and id.
14.	Based on the information from the previous step, the MQTT broker generates a topic structure for the end device and publishes the topics to the __\<dev_id\>/topics__.
15.	In this step, the MQTT end device subscribes to its topics, and all ongoing communication happens according to the MQTT2GO standard.



<p align="center" >
	<img src="mqtt_setup_cert.svg" alt="Proccess of adding a new MQTT2GO device">
</p>
<p align="center" >
	<em>Process of adding a new MQTT2GO device via guest WiFi.</em>
</p>

[Back](./index.md#add-devices)
