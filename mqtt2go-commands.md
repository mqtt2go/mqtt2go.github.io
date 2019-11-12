[Back](./index.md#data-structure)
# General Commands and Reports
<p align="justify">
The general commands and reports are used to communicate using device-independent messages. This means that all universal (common for all the devices) messages, such as errors and warnings are presented here. Furthermore, a general structure of command and report messages is given here. This structure is used for all MQTT2GO messages (both command and report ones).
</p>

## Topics Structure
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
To access a multiple devices or a whole group. Wildcard masks from the MQTT standard have to be used. If we want to substitute only one level, a <strong>+</strong> wildcard can be used. This means that the topic would look like:
</p>

```
<home_id>/<gateway_id>/+/<device_type>/<dev_id>
```

<p align="justify">
,which means that the subscribe/publish will be done to all groups, where the <strong>&lt;device_type&gt;/&lt;dev_id&gt;</strong> matches inserted data.
</p>

<p align="justify">
If the subscribe/publish should be to a larger group of end devices, a <strong>&#35;</strong> wildcard mask is used. This means that all topics after the <strong>&#35;</strong> are used:
</p>

```
<home_id>/<gateway_id>/#
```

<p align="justify">
,therefore means that the messages will go to all devices and all groups under selected gateway.
</p>


## MQTT Commands
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

## MQTT Reports
<p align="justify">
The report message structure is used for replies coming from the devices. The report messages can also contain a periodic update from the device. They are marked by the report type, which contains priority level to differentiate between critical reports (1) (such as alarms), replies to commands (2), and standard periodic messages (3).
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

[Back](./index.md#data-structure)
