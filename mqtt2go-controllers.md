# MQTT Controllers
<p align="justify">
MQTT Controllers are devices which are primarily serving as a controlling units. These devices can be either a physical device (touchscreen controller, smartphone, etc.) or a software controllers, which are accessible from the network (i.e., via HTTP).
</p>

## Topics Structure
<p align="justify">
The basic topic structure is based on the general MQTT2GO topic structure. The only difference here is that the controller can either control the end devices directly, or indirectly through groups and gateways.
</p>

```
<home_id>/<gateway_id>/<group_id>/<device_type>/<dev_id>
```

## MQTT Commands
<p align="justify">
The MQTT commands for the controllers are used to control the end devices. This means that most of the commands are targeted at setting up selected parameters of the end devices or changing the structure of the MQTT2GO households. Their structure is based on the  2.1.2.
</p>

### Set commands

The set commands are used to change / add device parameters and information. They are exactly the same as in Table with MQTT2GO commands with the addition of the set_group command type, which is used to set / change the group of selected end device. Its JSON body is:

```json
{
	"group_id": "group_id"
}
```

Where the __\<group_id\>__ can be either a value or field of values.

### Add Commands
<p align="justify">
The add commands are primarily used to create new objects like groups, users, and devices. They also utilize suffixes for differentiation. In the following paragraphs, all of the add commands will be described.
</p>

#### Group Creation
<p align="justify">
Group creation is done via <strong>add_group</strong> command type with value body of a JSON structure with the following format.	
</p>

```json
{
	"group_id": "group_id"
}
```

#### Add User
<p align="justify">
For adding a new user, the <strong>add_user</strong> command type with JSON body with following format is used.
</p>

```json
{
	"user_id": "id",
	"email": "email",
	"user_name": "name",
	"role": "role"
}
```

### Edit Commands
<p align="justify">
The edit commands are used to edit parameters of objects such as users, groups, and devices.
</p>

#### User Editing
<p align="justify">
User editing is done by commands with type of <strong>updt_use</strong> with JSON body of:
</p>

```json
{
	"user_id": "id",
	"email": "email",
	"user_name": "name",
	"role": "role"
}
```

#### Group Editing
<p align="justify">
Group editing is done by the commands with type of <strong>updt_group</strong> with JSON body of:
</p>

```json
{
	"group_id": "group_id"
}
```

### Delete Commands
<p align="justify">
Delete commands are primarily used to remove objects such as groups, devices, and users.Their suffixes are again based on the targeted functionality and are used as follows.
</p>

#### Remove Group
<p align="justify">
To remove a group, <strong>del_group</strong> command type is exploited and the format is of the JSON body is:
</p>

```json
{
	"group_id": "group_id"
}
```

where the __\<group_id\>__ can be either a single value or array.

#### Remove User
<p align="justify">
To remove a user, a <strong>del_user</strong> command type is used, with a JSON body of:
</p>

```json
{
	"user_id": "user_id"
}
```

#### Remove Device
<p align="justify">
To remove a device, a <strong>del_device</strong> command type is used. Its JSON body is:
</p>

```json
{
	"dev_id": "dev_id"
}
```

### Query Commands
<p align="justify">
The query commands utilized by the controllers are following the extension typology as the aforementioned set commands. The command type is therefore in form of <strong>query_querytype</strong>. They can be divided by their command type.
</p>

#### Query All Devices
<p align="justify">
Query all devices is used to request all available devices, its value will be <strong>all</strong>.
</p>

#### Query Device Info
<p align="justify">
Query device info requests information about selected device. Its value will be the selected <strong>dev_id</strong>.
</p>

#### Query User Info
<p align="justify">
Query user info is used to recall information about selected user. Its value field contains the selected <strong>user_id</strong>.
</p>

#### Query All Devices in Group
<p align="justify">
Query devices in group asks for all devices in specified group. Its value field is filled with <strong>group_id</strong>.
</p>

#### Query User's Topics
<p align="justify">
Query user’s topics requests the topics that selected user is subscribed to. Its value field contains the <strong>user_id</strong>.
</p>

## MQTT Reports
To Do

[Back](./)