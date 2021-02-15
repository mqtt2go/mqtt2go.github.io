[Back](../mqtt2go-objects.md)

# Smart Switch
To control the smart switches, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Turn on the switch
```json
{ 
    "type": "set",
    "timestamp": 1567677926,
    "value": "on"
}
```

### Turn off the switch
```json
{
    "type": "set",
    "timestamp": 1567677926,
    "value": "off"
}
```

## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### Turn on / off response

```json
{
    "type": "command_response",
    "timestamp": 1567677956,
    "value": "switch_value"
}
```

[Back](../mqtt2go-objects.md)