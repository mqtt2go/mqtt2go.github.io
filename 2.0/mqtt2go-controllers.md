[Back](./index.md#data-structure)
# MQTT Controllers
<p align="justify">
MQTT Controllers are devices which are primarily serving as a controlling units. These devices can be either a physical device (touchscreen controller, smartphone, etc.) or a software controllers, which are accessible from the network (i.e., via HTTP).
</p>

## Topics Structure
<p align="justify">
The basic topic structure is based on the general MQTT2GO topic structure. The only difference here is that the controller can either control the end devices directly, or indirectly through groups and gateways.
</p>

```
<home_id>/<gateway_id>/<dev_id>/<entity>/<msg_direction>
```

## <a name="controller-commands"></a>MQTT Commands
<p align="justify">
The MQTT commands for the controllers are used to control the end devices. This means that most of the commands are targeted at setting up selected parameters of the end devices or changing the structure of the MQTT2GO households. Their structure is based on the  <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>.
</p>

### Add Commands
<p align="justify">
The add commands are primarily used to create new objects like groups, users, rooms, and devices. They also utilize suffixes for differentiation. In the following paragraphs, all of the add commands will be described.
</p>

#### Add group

Group creation is done via <strong>group</strong> topic with <strong>create</strong> type and value body of a JSON structure with the following format.	

```
<home_id>/<gw_id>/group/in
```

```json
{
    "type": "create",
    "timestamp": "timestamp_value",
    "value": {
        "group_id": "group_id"
    }
}
```

Where the __\<group_id\>__ can be either a value or field of values.

#### Room Creation
<p align="justify">
Room creation is done via <strong>room</strong> topic with <strong>create</strong> type and value body of a JSON structure with the following format.
</p>

```
<home_id>/<gw_id>/room/in
```

```json
{
    "type": "create",
    "timestamp": "timestamp_value",
    "value": {
        "room_name": "room_name"	
    }
}
```

#### Scene Creation
<p align="justify">
Scene creation is done via <strong>scene</strong> topic with <strong>create</strong> type and value body of a JSON structure with the following format.
</p>

```
<home_id>/<gw_id>/scene/in
```

```json
{
    "type": "create",
    "timestamp": "timestamp_value",
    "value": {
        "scene_name": "scene_name"	
    }
}
```

#### Add User
<p align="justify">
For adding a new user, the <strong>&lt;home_id&gt;/in</strong> topic with command type of <strong>create_user</strong> with JSON body with following format is used.
</p>

```
<home_id>/in
```

```json
{
    "type": "create_user",
    "timestamp": "timestamp_value",
    "value": {
        "email": "email",
        "user_name": "name",
        "password": "pwd",
        "role": "role"
    }
}
```

#### Add Device to Group
<p align="justify">
For adding a new device to selected group, the <strong>add_device</strong> command type with JSON body with following format is used.
</p>

```
<home_id>/<gw_id>/group/in
```

```json
{
    "type": "add_device",
    "timestamp": "timestamp_value",
    "value": {
        "group_name": "group_name",
        "device_id": "device_id"
    }
}
```

#### Add Device to Scene
<p align="justify">
For adding a new device to selected scene, the <strong>add_device</strong> command type with JSON body with following format is used.
</p>

```
<home_id>/<gw_id>/scene/in
```

```json
{
    "type": "add_device",
    "timestamp": "timestamp_value",
    "value": {
        "device_id": "id",
        "scene_name": "scene_name"
    }
}
```

### Edit Commands
<p align="justify">
The edit commands are used to edit parameters of objects such as users, groups, and devices.
</p>

#### User Editing
<p align="justify">
User editing is done by commands with type of <strong>edit_user</strong> with JSON body of:
</p>

```
<home_id>/in
```

```json
{
    "type": "edit_user",
    "timestamp": "timestamp_value",
    "value": {
        "user_id": "id",
        "email": "email",
        "user_name": "name",
        "password": "pwd",
        "role": "role"
    }
}
```

#### Room Editing
<p align="justify">
Room editing is done by the commands with type of <strong>edit</strong> with JSON body of:
</p>

```
<home_id>/<gw_id>/room/in
```

```json
{
    "type": "edit",
    "timestamp": "timestamp_value",
    "value": {
        "room_id": "room_id",
        "room_name": "rooom_name"
    }
}
```

#### Scene Editing
<p align="justify">
Scene editing is done by the commands with type of <strong>edit</strong> with JSON body of:
</p>

```
<home_id>/<gw_id>/scene/in
```

```json
{
    "type": "edit",
    "timestamp": "timestamp_value",
    "value": {
        "scene_id": "scene_id",
        "scene_name": "scene_name"
    }
}
```

### Delete Commands
<p align="justify">
Delete commands are primarily used to remove objects such as groups, devices, and users.Their suffixes are again based on the targeted functionality and are used as follows.
</p>

#### Remove Group
<p align="justify">
To remove a group, <strong>delete</strong> command type is exploited and the format is of the JSON body is:
</p>

```
<home_id>/<gw_id>/group/in
```

```json
{
    "type": "delete",
    "timestamp": "timestamp_value",
    "value": {
        "group_id": "group_id"
    }
}
```

where the __\<group_id\>__ can be either a single value or array.

#### Remove User
<p align="justify">
To remove a user, a <strong>remove</strong> command type is used, with a JSON body of:
</p>

```
<home_id>/in
```

```json
{
    "type": "remove_user",
    "timestamp": "timestamp_value",
    "value": {
        "user_id": "user_id"
    }
}
```

### Query Commands
<p align="justify">
The query commands utilized by the controllers are following the extension typology as the aforementioned set commands. The command type is therefore in form of <strong>query_querytype</strong>. They can be divided by their command type.
</p>

#### Query All Devices
<p align="justify">
Query all devices is used to request all available devices, its value will be <strong>query_all</strong>.
</p>

```
<home_id>/<gw_id>/devices/in
```

```json
{
    "type": "query_all",
	"timestamp": "timestamp_value",
	"value": "all"
}
```

#### Query User Info
<p align="justify">
Query user info is used to recall information about selected user. Its value field contains the selected <strong>user_id</strong>.
</p>

```
<home_id>/in
```

```json
{
    "type": "user_info",
    "timestamp": "timestamp_value",
    "value": "user_id"
}
```

#### Query All Devices in Group
<p align="justify">
Query all devices in specified group. Its value field is filled with <strong>group_id</strong>.
</p>

```
<home_id>/<gw_id>/group/in
```

```json
{
    "type": "query_all",
    "timestamp": "timestamp_value",
    "value": "group_id"
}
```

#### Query All Groups
<p align="justify">
Query devices in group asks for all groups. Its value field is filled with <strong>group_id</strong>.
</p>


```
<home_id>/<gw_id>/group/in
```

```json
{
    "type": "query_all",
    "timestamp": "timestamp_value"
}
```

## MQTT Reports
The MQTT reports utilized here are mostly a replies to the commands from the controlled devices. They follow the general report structure and add a specific report names. The report <strong>value</strong> can be either a simple OK, or a body JSON of following structure:

```
<home_id>/<gw_id>/devices/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value": "value"
}
```

, where <strong>event_name</strong> can be warning or error.

### Add Reports
The add reports are generally used to report the result of add commands. They will be describing in following subsections.

#### Group Creation Report
After the add_group command is received. The device process it and then publishes following message to the same topic:

```
<home_id>/<gw_id>/group/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":"value" 
}
```

#### Add User Report
After the add_user command is received, the devices processes it and then publishes following message into the same topic from which the command came:

```
<home_id>/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":{
        "event" : "event_value",
        "reason" : "reason_value"
    }
}
```

### Edit Reports
The edit reports are used to report the outcome of edit commands.

#### User Editing report
After the updt_user command is issues, the following response is published into the same topic:

```
<home_id>/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":"value" 
}
```

#### Group Editing Report
After the updt_group command is issues, the following response is published into the same topic:

```
<home_id>/<gw_id>/group/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":"value" 
}
```

### Delete Reports
The delete reports are the primary outcome of the events started by delete commands. Their message structure also follows the general report structure. Its value follows the same pattern as the general MQTT report, described in this chapter. Therefore the value can be either a simple OK, or a JSON body with the event name and description.

#### Remove Group
Is the report which is published after the del_group command is issued. 

```
<home_id>/<gw_id>/group/out
```


```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":"value" 
}
```

#### Remove User
Is the report which is published after the del_user command is issued. 


```
<home_id>/<gw_id>/user/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp": "timestamp_value",
    "value": "value" 
}
```

### Query Reports
Query reports are the most bulky reports from this chapter. They contain a response that is awaited by the controlled after issuing a query command. The query command is usually used to force important device information.

#### Query All Devices Report
Is used to return all devices under selected SH-GW. It has following structure:

```
<home_id>/<gw_id>/devices/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":[ "device_1_id", "device_2_id", ...]
}
```

The value of this report contains a field of all available devices ids.

#### Query All Groups Report
Is used to return all devices under selected SH-GW. It has following structure:

```
<home_id>/<gw_id>/group/out
```

```json
{
    "type": "command_response",
	"priority_level": 2,
	"timestamp":"timestamp_value",
	"value":[ "group_1_id", "group_2_id", ...]
}
```

#### Query User Info Report
This report provides information about the user. This report is invoked by the <strong>query_user_info</strong> command. Its structure is following:

```
<home_id>/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value": {
        "user_id": "user_id",
        "email": "john.doe@mail.com",
        "user_name": "John Doe",
        "role": "administrator"
    }
}
```

#### Query All Devices in Group Report
Is used to return all devices in specified group. It has following structure:

```
<home_id>/<gw_id>/group/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":[ "device_1_id", "device_2_id", ...]
}
```
The value of this report contains a field of ids for all devices in specified group.

#### Query User's Topics Report
This report contains all topics of specified user. Its structure is following:

```
<home_id>/out
```

```json
{
    "type": "command_response",
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value":[ "topic1", "topic2",...]
}
```
The value can contain a simple topic or a field of topics, as illustrated above.

### MQTT2GO Event Topic
MQTT2GO standard also defines a special topic for designated for logging all events happening inside the smart home. This topic is:

```
<home_id>/<gateway_id>/events/<msg_direction>
```

Its primary goal is to cumulate all important events at one place. It can be utilized for example by machine-learning controller, that will be sending events about unusual activity into this topic. The structure of the event message is as follows:

```json
{
    "priority_level": 2,
    "timestamp":"timestamp_value",
    "value": {
        "event_name": "event_name",
        "message": "message"
    }
}
```

[Back](./index.md#data-structure)
