[Back](../mqtt2go-objects.md)

# Blinds and Sunscreens
To control the blinds and sunscreens, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.



### Set Target Position
```
<home_id>/<gw_id>/<device_id>/position/in
```
```json
{
	"type": "set",
	"timestamp":1567677926,
	"value": 50
}
```

### Set Up Position
```
<home_id>/<gw_id>/<device_id>/position/in
```
```json
{
	"timestamp": 1567677926,
	"value": 100
}
```

### Set Down Position
```
<home_id>/<gw_id>/<device_id>/position/in
```
```json
{
	"timestamp": 1567677926,
	"value": 0
}
```

### Stop Position
```
<home_id>/<gw_id>/<device_id>/control/in
```
```json
{
	"timestamp": 1567677926,
	"value": "stop"
}
```
### Set Horizontal Tilt Angle
```
<home_id>/<gw_id>/<device_id>/htilt/in
```
```json
{
    "timestamp": 1567677926,
    "value": 90
}
```

### Set Vertical Tilt Angle
```
<home_id>/<gw_id>/<device_id>/vtilt/in
```
```json
{
    "timestamp": 1567677926,
    "value": 90
}
```
### Set Timer
The set_timer command is utilized to setup the periodical action which will trigger at the time set via the time value. In this case, the device will trigger the up/down action at the selected time.
```
<home_id>/<gw_id>/<device_id>/timer/in
```
```json
{
	"timestamp":1567677926,
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
```
<home_id>/<gw_id>/<device_id>/position/out
```
```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": 0
}
```

```
<home_id>/<gw_id>/<device_id>/control/out
```
```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "stop"
}
```

```
<home_id>/<gw_id>/<device_id>/maintenance/out
```
```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "repair_needed"
}
```

### Current Position Report
```
<home_id>/<gw_id>/<device_id>/position/out
```
```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": 50
}
```

### Current Horizontal Angle Report
```
<home_id>/<gw_id>/<device_id>/htilt/out
```
```json
{
	"type": "report_type",
    "priority_level": 2,
    "timestamp": 1567677956,
    "report": 90
}
```

### Current Vertical Angle Report
```
<home_id>/<gw_id>/<device_id>/vtilt/out
```
```json
{
	"type": "report_type",
    "priority_level": 2,
    "timestamp": 1567677956,
    "report": 90
}
```

### Obstruction Detected Report
```
<home_id>/<gw_id>/<device_id>/obstruction/out
```
```json
{
	"type": "report_type",
	"priority_level": 1,
	"timestamp": 1567677956,
	"report": "true"
}
```
[Back](../mqtt2go-objects.md)
