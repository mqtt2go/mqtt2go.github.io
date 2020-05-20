[Back](../mqtt2go-objects.md)

# Smart Sockets
To control the smart sockets, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Set On
```json
{
	"timestamp": 1567677926,
	"value": "on"
}
```

### Set Off
```json
{
	"timestamp": 1567677926,
	"value": "off"
}
```

### Set Timer
The set_timer command is utilized to setup the periodical action which will trigger at the time set via the time value. In this case, the device will turn on/off at the selected time.

```json
{
	"timestamp": 1567677926,
	"value": {
		"unit": "hh:mm:ss",
		"time": "02:12:00",
		"action": "on/off"
	}
}
```

## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### State Report

### Consumption Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value": 45,
		"unit": "Ws"
	}
}
```

### Current Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value": 1.2,
		"unit": "A"
	}
}
```

### Voltage Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value": 232.4,
		"unit": "V"
	}
}
```

### Power Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value": 90,
		"unit": "W"
	}
}
```


[Back](../mqtt2go-objects.md)