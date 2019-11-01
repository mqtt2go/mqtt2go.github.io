
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

[Back](./)
