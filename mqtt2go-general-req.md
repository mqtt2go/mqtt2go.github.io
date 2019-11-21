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
## System Requirements (Cloud)
| ID | Status | Description                                                                                  |
|----|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| C1 | MUST   | The selected operating system must enable installation of the below mentioned packages which are requested for the functionality of the MQTT2GO standard.|
| C2 | MUST   | List of required functionality enabled by installed packages: multicast DNS (mDNS); WiFi Protected Setup (WPS); Secure Sockets Layer (SSL); Transport Layer Security (TLS); MQTT Broker.|

## System Requirements (MQTT Controllers)
| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| M1 | MUST   | The selected operating system must enable installation of the below mentioned packages which are requested for the functionality of the MQTT2GO standard.|
| M2 | MUST   | List of required functionality enabled by installed packages: Secure Sockets Layer (SSL); Transport Layer Security (TLS); MQTT Client.|
| M2 | MUST   | List of optional packages based on the communication scenario: Wifi Protected Setup (WPS).|

## Hardware Requirements (MQTT Controllers)
| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| N1 | MUST   | The device in the role of the MQTT controller must enable communication with the Management Server via the wired or wireless communication interface, see Section [Setup new MQTT2GO Controller](./add-controller.md).|

## System Requirements (Local Gateway)
| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| L1 | MUST   | The selected operating system must enable installation of the below mentioned packages which are requested for the functionality of the MQTT2GO standard.|
| L2 | MUST   | List of required functionality enabled by installed packages: multicast DNS (mDNS); WiFi Protected Setup (WPS); Secure Sockets Layer (SSL); Transport Layer Security (TLS); MQTT Broker; MQTT Client.|
| L3 | CAN   | List of optional packages based on the communication scenario (with or without the local MQTT broker): MQTT Client.|

## Hardware Requirements (Local Gateway)
| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| H1 | MUST   | The device in the role of the local SH-GW must have 2x SSID Wi-Fi 2.4GHz + 5GHz (IEEE 802.11b/g/n and 802.11a/n/ac) network, but SHOULD be more. The SH-GW must support the WPS functionality (as mentioned above) as it is related to the proposed functionality of the local MQTT broker.|
| H2 | MUST   | Sufficient amount of FLASH memory to run the solution implemented based on the MQTT2GO standard.|
| H2 | MUST   | Sufficient amount of RAM memory to run the solution implemented based on the MQTT2GO standard.|




[Back](./index.md#requirements)
