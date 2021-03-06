[Back](./index.md#requirements)
# MQTT2GO Broker Requirements
This section is related to the requirements for a Smart-Home Gateway (SH-GW) in the role of the local MQTT broker, see the MQTT System Architecture. Not only the SH-GW is compliant with the software requirements given by the MQTT2GO standard i.e., acting as a local MQTT broker, MQTT proxy, and wireless access bridge, it must be also able to run 3rd party applications (in a container (SH namespace) as virtual environment) in a completely isolated environment and ensure high portability and security. 

## Requirement Types

| Status | Description                                                 |
|--------|-------------------------------------------------------------|
| Status | Description                                                 |
| MUST   | Mandatory requirement.                                      |
| SHOULD | Requirement which should be met, but which is not absolute. |
| CAN    | Optional requirement which will strengthen the offering.    |

## Minimal Functionality Requirements on the SH-GW
### Core Requirements

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| C1 | MUST   | Linux platform. ARM or MIPS SoC families supported by OpenWrt Project (https://openwrt.org). |
| C2 | MUST   | Firmware image based on latest stable version of OpenWrt Linux distribution image with LXC enabled, templates and configured namespaces with root accounts. |
| C3 | MUST   | At least two usable SH namespace processor cores from the CPUs of the host device with sufficient performance. |
| C4 | MUST   | Sufficient free FLASH memory for the SH namespace. At least 256 MB of FLASH memory with minimal 20 MB available for the user. |
| C5 | MUST   | Sufficient free RAM for the SH namespace. At least 512 MB RAM.|
| C6 | MUST   | 2 x SSID Wi-Fi 2.4 GHz + 5 GHz (IEEE 802.11b/g/n and 802.11a/n/ac), but SHOULD be more. The SH-GW must support the WPS functionality as it is related to the proposed functionality of the local MQTT broker.|

### Developer support requirements

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| D1 | MUST   | Comprehensive SDK with complete documentation, code samples, toolchain, etc. |
| D2 | MUST   | Provision of end-user firmware versions corresponding to the functionality of the Reference SDK devices (see D1). |
| D3 | MUST   | List of the available packages within the provided SDK -- in case the version of the OpenWRT is no longer supported and the developers are forced to stick only with the packages within the SDK. |

### Installed Packages

| ID | Status | Description                                                                                  |
|----|--------|----------------------------------------------------------------------------------------------|
| P1 | MUST | Python 2.7 or 3.x.                                                                         |
| P2 | MUST | Kernel modules, binaries and iptables extensions to include: conntrack; ebtables; iptables-mod-conntrack-extra; iptables-mod-ipmark; iptables-mod-nflog; iptables-mod-nfqueue; kmod-br-netfilter; libopenssl; ca-certificates; curl |
| P3 | SHOULD | APIs or iptables rules to control hardware acceleration. |
| P4 | SHOULD | LXC Containers are run in the same Network Space as the HOST for proper traffic steering. |
| P5 | SHOULD | LXC Containers have to have an ability to run iptables commands. |

[Back](./index.md#requirements)
