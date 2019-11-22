[Back](./index.md#requirements)
# MQTT2GO General Requirements
In this section the requirements which have to be fulfilled in order to work with the proposed MQTT2GO standard. The requirements are logically divided into groups based on their type.

| Status | Description                                                 |
|--------|-------------------------------------------------------------|
| Status | Description                                                 |
| MUST   | Mandatory requirement.                                      |
| SHOULD | Requirement which should be met, but which is not absolute. |
| CAN    | Optional requirement which will strengthen the offering.    |

# Cloud Requirements
## System Requirements

| ID | Status | Description                                                                                  |
|----|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| C1 | MUST   | The selected operating system must enable installation of the below mentioned packages which are requested for the functionality of the MQTT2GO standard.|
| C2 | MUST   | List of required functionality enabled by installed packages: multicast DNS (mDNS); WiFi Protected Setup (WPS); Secure Sockets Layer (SSL); Transport Layer Security (TLS v1.2 or higher); MQTT Broker.|

# MQTT Controllers Requirements
## System Requirements

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| M1 | MUST   | The selected operating system must enable installation of the below mentioned packages which are requested for the functionality of the MQTT2GO standard.|
| M2 | MUST   | List of required functionality enabled by installed packages: Transport Layer Security (TLS v1.2 or higher; including SSLv3); MQTT Client.|
| M3 | MUST   | List of optional packages based on the communication scenario: WiFi Protected Setup (WPS).|

## Hardware Requirements

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| N1 | MUST   | The device in the role of the MQTT controller must enable communication with the Management Server via the wired or wireless communication interface, see Section [Setup new MQTT2GO Controller](./add-controller.md).|

# Local Gateway Requirements
## System Requirements 

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| L1 | MUST   | The selected operating system must enable installation of the below mentioned packages which are requested for the functionality of the MQTT2GO standard.|
| L2 | MUST   | List of required functionality enabled by installed packages: multicast DNS (mDNS); WiFi Protected Setup (WPS); Transport Layer Security (TLS v1.2 or higher; including SSLv3); MQTT Broker; MQTT Client (v3.1.1 or newer).|
| L3 | CAN   | List of optional packages based on the communication scenario (with or without the local MQTT broker): MQTT Client MQTT Client (v3.1.1 or newer).|

## Hardware Requirements 

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| H1 | MUST   | The device in the role of the local SH-GW must have 2x SSID Wi-Fi 2.4GHz + 5GHz (IEEE 802.11b/g/n and 802.11a/n/ac) network, but SHOULD be more. The SH-GW must support the WPS functionality (as mentioned above) as it is related to the proposed functionality of the local MQTT broker.|
| H2 | MUST   | Sufficient amount of FLASH memory to run the solution implemented based on the MQTT2GO standard.|
| H3 | MUST   | Sufficient amount of RAM memory to run the solution implemented based on the MQTT2GO standard.|

# MQTT Objects
## System Requirements

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| O1 | MUST   | Out of the box, the end-device must act as a MQTT client; the implementation of the MQTT v3.1.1 or newer must be done.|
| O2 | MUST   | List of required functionality: multicast DNS (mDNS); WiFi Protected Setup (WPS); Transport Layer Security (TLS v1.2 or higher; including SSLv3).|
| O3 | MUST   | In case the end-device does not implement MQTT messaging protocol, the intermediate gateway must be provided to translate the communication between local MQTT broker and the end device.|


## Hardware Requirements 

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| P1 | MUST   | The end-devices in the role of the MQTT Objects that are connected to the MQTT2GO system i.e., to the local or cloud MQTT broker must be equipped with a QR code for the initial connection setup. |


[Back](./index.md#requirements)
