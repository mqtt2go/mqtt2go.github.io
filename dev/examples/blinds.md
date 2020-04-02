[Back](../mqtt2go-objects.md)

# Blinds and Sunscreens
To control the blinds and sunscreens, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Set Target Position
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set_target_position",
	"value": 50
}
```

### Set Up Position
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set",
	"value": "up"
}
```

### Set Down Position
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set",
	"value": "down"
}
```

### Stop Position
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set",
	"value": "stop"
}
```

### Set Horizontal Tilt Angle
```json
{
    "type": "command",
    "timestamp": 1567677926,
    "command_type": "set_horizontal_tilt",
    "value": {
        "unit": "degree",
        "angle": 90
    }
}
```

### Set Vertical Tilt Angle
```json
{
    "type": "command",
    "timestamp": 1567677926,
    "command_type": "set_vertical_tilt",
    "value": {
            "unit": "degree",
            "angle": 90
    }
}
```
### Set Timer
The set_timer command is utilized to setup the periodical action which will trigger at the time set via the time value. In this case, the device will trigger the up/down action at the selected time.

```json
{
	"type": "command",
	"timestamp":1567677926,
	"command_type": "set_timer",
	"value": {
            "unit": "hh:mm:ss",
            "time": "01:12:00",
            "action": "up/down"
    }
}
```

## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### State Report
```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "command_response",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "closed"
}
```

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "command_response",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "open"
}
```

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "command_response",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "stopped"
}
```

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "command_response",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "repair_needed"
}
```

### Current Position Report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "command_response",
	"timestamp": 1567677956,
	"report_name": "current_position",
	"report": 50
}
```

### Current Horizontal Angle Report

```json
{
    "type": "report",
    "priority_level": 2,
    "report_type":"command_response",
    "timestamp": 1567677956,
    "report_name": "current_horizontal_angle",
    "report": {
        "unit": "degree",
        "h": 90
    }
}
```

### Current Vertical Angle Report

```json
{
    "type": "report",
    "priority_level": 2,
    "report_type": "command_response",
    "timestamp": 1567677956,
    "report_name": "current_vertical_angle",
    "report": {
        "unit": "degree",
        "h": 90
    }
}
```

### Obstruction Detected Report

```json
{
	"type": "report",
	"priority_level": 1,
	"report_type": "status",
	"timestamp": 1567677956,
	"report_name": "obstruction_detected_report",
	"report": "true"
}
```
[Back](../mqtt2go-objects.md)
