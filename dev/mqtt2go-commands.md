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
 <li>where the <strong>&lt;home_id&gt;</strong> stands for the unique identificator of the home (this is used for the identification of a group of users, that are sharing one or more SH-GWs and corresponding amount of devices connected to them),</li>
 <li><strong>&lt;gateway_id&gt;</strong> is the unique identificator of the SH-GW,</li>
 <li><strong>&lt;dev_id&gt;</strong> is device’s own unique identificator,</li>
 <li><strong>&lt;entity&gt;</strong> is the unique identificator of the message information (i.e., humidity),</li>
 <li>and <strong>&lt;msg_direction&gt;</strong> defines the direction of the communication, where <strong>in</strong> stands for the communication going to the device (i.e., commands) and <strong>out</strong> represents the responses from the device to the gateway / controller.</li>
 </ul>


<p align="justify">
To access a multiple devices or all of their entities, wildcard masks from the MQTT standard have to be used. If we want to substitute only one level, a <strong>+</strong> wildcard can be used. This means that the topic would look like:
</p>

```
<home_id>/<gateway_id>/+/<entity>/<msg_direction>
```

<p align="justify">
which means that the subscription will be done to all devices, where the <strong>&lt;entity&gt;/&lt;msg_direction&gt;</strong> matches inserted data.
</p>

<p align="justify">
If the subscription should be to a larger group of end devices, a <strong>&#35;</strong> wildcard mask is used. This means that all topics starting at the <strong>&#35;</strong> and further are subscribed:
</p>

```
<home_id>/<gateway_id>/#
```

Some examples of the whole topic structure are as follows:

* Information topic of a device:

```
<home_id>/<gateway_id>/<dev_id>/about/<msg_direction>
```

* Topics used to switch on/off either the device or its relay and to report the status from the device back to the controller:

```
<home_id>/<gateway_id>/<dev_id>/switch/<msg_direction>
```

* Topic utilized for the humidity reports

```
<home_id>/<gateway_id>/<dev_id>/humidity/out
```


## <a name="mqtt_commands"></a>MQTT Commands
<p align="justify">
The commands are composed of three fields: (i) <strong>type</strong>, providing information about the type of the command, (ii) <strong>timestamp</strong>, and (iii) <strong>value</strong> containing the actual command. The command itself can be either a simple name-value pair or a complex structure, which is usually used for complex operations such as device setup.
</p>

```json
{
    "type": "command_type",
    "timestamp": "timestamp_value",
    "value": "value_body"
}
```
<p align="justify">
The <strong>timestamp</strong> defines the datetime of the event sent within the message. It is in Unix format.
The <strong>type</strong> defines what information should be expected in the <strong>value</strong> field. It can be any of the command types defined in the sections <a href="./mqtt2go-objects#object-commands">Objects MQTT Commands</a> and <a href="./mqtt2go-controllers#controller-commands">Controllers MQTT Commands</a>. For example, if the <strong>command_type_value</strong> contains <strong>set</strong>, a value of simple commands such as <strong>on</strong> can be expected. Id addition, if <strong>command_type_value</strong> contains a <strong>color</strong> keyword, the value should contain an array, which describes the RGB information needed to set up the chosen color.<br>
Based on previous examples, the <strong>value</strong> field can contain either a simple command such as <strong>on, off</strong> and similar, or more advanced commands represented by an array (i.e., the array for RGB information for setting the light color).
</p>

The general query commands common for all devices are identified by the **query_command** and defined as follows: 

* A topic used for controlling the **on/off** status of the device (below, the on example is shown):

```
<home_id>/<gateway_id>/<dev_id>/switch/<msg_direction>
```

* A topics used to query the battery level and report the value back:

```
<home_id>/<gateway_id>/<dev_id>/battery/<msg_direction>
```
* A topic used to query current state of the device and report the value back:

```
<home_id>/<gateway_id>/<dev_id>/state/<msg_direction>
```
* A topic to query the tamper status of selected device and report the value back:

```
<home_id>/<gateway_id>/<dev_id>/tamper/<msg_direction>
```
* A topic used to query the status of the device and report the value back:

```
<home_id>/<gateway_id>/<dev_id>/status/<msg_direction>
```

## <a name="mqtt_reports"></a>MQTT Reports
<p align="justify">
The report message structure is used for replies coming from the devices. The report messages can also contain a periodic update from the device. They are marked by the report type, that helps to differentiate between aforementioned report types. Furthermore, the priority level field enables devices to divide messages by their priority (1) (such as alarms), replies to commands (2), and standard periodic messages (3).
</p>

```json
{
    "type": "report_type_value",
    "priority_level":"priority_level_value",
    "timestamp":"timestamp_value",
    "value":"value_body" 
}
```

<p align="justify">
The <strong>priority_level</strong> is used to set a message priority. It can be between 1-4, where 1 denotes the lowest and 5 the highest.
The report_type defines the type of report, there are four types of reports:</p>

1. Status,
2. CommandResponse,
3. PeriodicReport,
4. Error.

<p align="justify">
The timestamp defines the datetime of the message sent event. It is in Unix format.

[Back](./index.md#data-structure)
