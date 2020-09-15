[Back](../mqtt2go-objects.md)

# Smart Lights
To control the smart lights, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Turn on the light
```json
{ 
    "type": "set",
    "timestamp": 1567677926,
    "value": "on"
}
```

### Turn off the light
```json
{
    "type": "set",
    "timestamp": 1567677926,
    "value": "off"
}
```

### Set Color
The color function is utilized to set desired color to the light / bulb. 

```json
{
    "type": "color",
    "timestamp": 1567677926,
    "value": {
        "r": 125,
        "g": 0,
        "b": 65
    }
}
```

### Set brightness

```json
{
    "type": "brightness",
    "timestamp": 1567677926,
    "value": 75
}
```

### Set color temperature

```json
{
    "type": "color_temp",
    "timestamp": 1567677926,
    "value": 50 
}
```

## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### State Report

### Turn on / off response

```json
{
    "type": "command_response",
    "timestamp": 1567677956,
    "value": "switch_value"
}
```

### Set color response

```json
{
    "type": "command_response",
    "timestamp": 1567677956,
    "value": {
        "r": 255, 
        "g": 0,
        "b": 0
    }
}
```

### Set brightness response

```json
{
    "type": "command_response",
    "timestamp": 1567677956,
    "value": 80
}
```

### Set color temperature response

```json
{
    "type": "color_temp",
    "timestamp": 1567677926,
    "value": 50
}
```

[Back](../mqtt2go-objects.md)