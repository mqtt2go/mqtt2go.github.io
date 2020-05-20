[Back](./index.md#data-structure)
# User Management
<p align="justify">
This section describes the topics that deal with user management. That being said, the core functionality of this module is to add, delete or update users inside a one MQTT2GO smart-home (which does not necessarily mean one SH-GW). We distinguish between three user roles: (i) Administrator, who has full access to all features and therefore can control whole household without any restrictions, (ii) family member, who can control the whole smart-home but cannot manage users, and guest, who can control only utility home devices, such as lights nad smart TVs.
</p>

[logo]: https://github.githubassets.com/images/icons/emoji/unicode/274c.png?v8&s=10 "Cross"

| User Role                   | Administrator | Family member | Guest |
|-----------------------------|:---------------:|:---------------:|:-------:|
| User management             | ✔               | 🗙              | 🗙      |
| Security device control     | ✔ | ✔ | 🗙 |
| Utility home device control | ✔ | ✔ | ✔  |

## Topics Structure
The base topic for user management is defined as: 

```
<home_id>/users/<command_type>
```

<p align="justify">
Inside this topic, the MQTT2GO commands are sent if any user-related operation is to be performed.
</p>

## MQTT Commands
<p align="justify">
The MQTT commands for user management are based on the commands template from <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>, where the MQTT <strong>command_type</strong> will be one of the following:
</p>

* add_user,
* updt_user
* del_user.

The JSON body for first two commands will be:

```json
{
	"user_id": "id",
	"email": "email",
	"user_name": "name",
	"role": "role"
}
```

and for the deletion operation:

```json
{
	"user_id": "id"
}
```

The concrete value of each parameter depends on the specific use-case.

## MQTT Reports
<p align="justify">
The MQTT reports for the user management are based on the commands template from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a>, where the MQTT report_type corresponds to the commands from: MQTT Commands, with the JSON body containing the operation results:
</p>

```json
{
	"result": "result"
}
```

The result itself can be:
* OK,
* Error.

## Examples
The example of a user add operation can look like:

```json
{
	"user_id": 123456789,
	"email": "john.doe@mail.com",
	"user_name": "John Doe",
	"role": "adult"
}
```

and the response will be:

```json
{
	"result": "ok" 
}
```

[Back](./index.md#data-structure)
