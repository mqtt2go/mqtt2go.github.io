
# MQTT2GO Data Structure
This section is dedicated to the MQTT2GO topic naming convention. The structure is organized into subsections based on the device types to provide an easier understanding of the division and key roles of each entity inside the MQTT2GO standard.

## General Commands and Reports
The general commands and reports are used to communicate device-independent messages. This means that all universal (common for all the devices) messages, such as errors and warnings are presented here. Furthermore, a general structure of command and report messages is given here. This structure is used for all MQTT2GO messages (both command and report ones).

### Topics Structure
The general MQTT2GO topic structure is created to be as efficient as possible, generating a reasonable amount of subtopics and therefore following the MQTT best practices. The topic itself is then composed as follows:

Topic: <home_id>/<gateway_id>/<group_id>/<device_type>/<dev_id>,

where the <home_id> stands for the unique identificator of the home (this is used for the identification of a group of users, that are sharing one or more gateways and corresponding amount of devices connected to them),
<gateway_id> is the unique identificator of the gateway,
<group_id> is the unique identificator of the group of devices,
<device_type> defines to which category the device belongs,
and <dev_id> is device’s own unique identificator.


[Back](./)