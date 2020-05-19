[Back](./index.md#data-structure)
# General Commands and Reports
<p align="justify">
The general commands and reports are used to communicate using device-independent messages. This means that all universal (common for all the devices) messages, such as errors and warnings are presented here. Furthermore, a general structure of command and report messages is given here. This structure is used for all MQTT2GO messages (both command and report ones).
</p>

## <a name="mqtt_topics"></a>Topics Structure
<p align="justify">
The general MQTT2GO topic structure is created to be as efficient as possible, generating a reasonable amount of subtopics and therefore following the MQTT best practices. The main topic structure itself is then composed as follows:
</p>

```
<home_id>/<gateway_id>/<dev_id>/<entity>/<msg_direction>
```


<ul>
 <li>where the <strong>&lt;home_id&gt;</strong> stands for the unique identificator of the home (this is used for the identification of a group of users, that are sharing one or more gateways and corresponding amount of devices connected to them),</li>
 <li><strong>&lt;gateway_id&gt;</strong> is the unique identificator of the gateway,</li>
 <li><strong>&lt;dev_id&gt;</strong> is device’s own unique identificator,</li>
 <li><strong>&lt;entity&gt;</strong> is the unique identificator of the message information (i.e., humidity),</li>
 <li>and <strong>&lt;msg_direction&gt;</strong> defines the direction of the communication, where <strong>in</strong> stands for the communication going to the device (i.e., commands) and <strong>out</strong> represents the responses from the device to the gateway / controller.</li>
 </ul>


<p align="justify">
To access a multiple devices or all of their entities. Wildcard masks from the MQTT standard have to be used. If we want to substitute only one level, a <strong>+</strong> wildcard can be used. This means that the topic would look like:
</p>

```
<home_id>/<gateway_id>/+/<entity>/<msg_direction>
```

<p align="justify">
,which means that the subscribe will be done to all devices, where the <strong>&lt;entity&gt;/&lt;msg_direction&gt;</strong> matches inserted data.
</p>

<p align="justify">
If the subscribe should be to a larger group of end devices, a <strong>&#35;</strong> wildcard mask is used. This means that all topics after the <strong>&#35;</strong> are used:
</p>

```
<home_id>/<gateway_id>/#
```

<p align="justify">
,therefore means that the messages will go to all devices and all groups under selected gateway.
</p>

Some examples of the whole topic structure are as follows:

* Information topic of a device:

```
<home_id>/<gateway_id>/<dev_id>/about/in
```

* Topic used to switch on/off either the device or its relay:

```
<home_id>/<gateway_id>/<dev_id>/power/in
```

* Topic utilized for the humidity reports

```
<home_id>/<gateway_id>/<dev_id>/humidity/out
```


## <a name="mqtt_commands"></a>MQTT Commands
<p align="justify">
The command messages are composed of two fields: (i) <strong>timestamp</strong> and (ii) <strong>value</strong> structure, containing the actual command. The command itself can be either a simple name-value pair or a complex structure, which is usually used for complex operations such as device setup.
</p>

```json
{
	"timestamp": "timestamp_value",
	"value": "value_body"
}
```
<p align="justify">
The <strong>timestamp</strong> defines the datetime of the message sent event. It is in Unix format.
The <strong>command_type</strong> defines what information should be expected in the <strong>value</strong> key-pair. It can be any of the command types defined in the sections <a href="./mqtt2go-objects#object-commands">Objects MQTT Commands</a> and <a href="./mqtt2go-controllers#controller-commands">Controllers MQTT Commands</a>. If the <strong>command_type_value</strong> will contain <strong>set</strong>, a value of simple commands such as <strong>on</strong> can be expected. If <strong>command_type_value</strong> will contain a <strong>color</strong> keyword, the value will contain an array, which will describe the HSB information needed to set up the chosen color.<br>
Based on previous examples, the <strong>value</strong> key-pair can contain either a simple command such as <strong>on, off</strong> and similar, or more advanced commands represented by an array (i.e., the array for HSB information for setting the light color).
</p>

The general query commands that are common for all devices are as follows: 

* A topics used for controlling the **on/off** status of the device (the on is in the example):

```
<home_id>/<gateway_id>/<dev_id>/on/in
```

* A topic used to query the battery level:

```
<home_id>/<gateway_id>/<dev_id>/battery/in
```
* A topic used to query current state of the device

```
<home_id>/<gateway_id>/<dev_id>/state/in
```
* A topic to query the tamper status of selected device:

```
<home_id>/<gateway_id>/<dev_id>/tamper/in
```
* A topic used to query the status of the device:

```
<home_id>/<gateway_id>/<dev_id>/status/in
```

## <a name="mqtt_reports"></a>MQTT Reports
<p align="justify">
The report message structure is used for replies coming from the devices. The report messages can also contain a periodic update from the device. They are marked by the report type, that helps to differentiate between aforementioned report types. Furthermore, the priority level field enables devices to divide messages by their priority (1) (such as alarms), replies to commands (2), and standard periodic messages (3).
</p>

```json
{
	"priority_level":"priority_level_value",
	"timestamp":"timestamp_value",
	"report_name":"report_name",
	"value":"value_body" 
}
```

<p align="justify">
The <strong>priority_level</strong> is used to set a message priority. It can be between 1-5, where 1 is the lowest and 5 the highest.
The report_type defines the type of report, there are four types of reports:</p>

1. Status,
2. CommandResponse,
3. PeriodicReport,
4. Error.

<p align="justify">
The timestamp defines the datetime of the message sent event. It is in Unix format.
The <strong>report_name</strong> carries information about the value format. It can be any of the names from the afore presented tables. For example, when the <strong>report_name</strong> has a value of temperature, the value key-pair will carry an integer which corresponds to a degrees of celsius.
<strong>value_body</strong> can be either a simple response (OK), an integer, or array of values.
</p>

The general reports that are common for all devices are:
* OK,
* BatteryBad.

[Back](./index.md#data-structure)
