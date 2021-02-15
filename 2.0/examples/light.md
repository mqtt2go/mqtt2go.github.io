[Back](../mqtt2go-objects.md)

# Smart Lights
To control the smart lights, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Turn on the light
```
<home_id>/<gateway_id>/<device_id>/switch/in
```
```json
{ 
    "type": "set",
    "timestamp": 1567677926,
    "value": "on"
}
```

### Turn off the light
```
<home_id>/<gateway_id>/<device_id>/switch/in
```
```json
{
    "type": "set",
    "timestamp": 1567677926,
    "value": "off"
}
```

### Set Color
The color function is utilized to set desired color to the light / bulb. 
```
<home_id>/<gateway_id>/<device_id>/color/in
```
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
```
<home_id>/<gateway_id>/<device_id>/brightness/in
```
```json
{
    "type": "brightness",
    "timestamp": 1567677926,
    "value": 75
}
```

### Set color temperature
```
<home_id>/<gateway_id>/<device_id>/temperature/in
```
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
```
<home_id>/<gateway_id>/<device_id>/switch/out
```
```json
{
    "type": "command_response",
    "timestamp": 1567677956,
    "value": "on/off"
}
```

### Set color response
```
<home_id>/<gateway_id>/<device_id>/color/out
```
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
```
<home_id>/<gateway_id>/<device_id>/brightness/out
```
```json
{
    "type": "command_response",
    "timestamp": 1567677956,
    "value": 80
}
```

### Set color temperature response
```
<home_id>/<gateway_id>/<device_id>/temperature/out
```
```json
{
    "type": "color_temp",
    "timestamp": 1567677926,
    "value": 50
}
```

[Back](../mqtt2go-objects.md)