[Back](../mqtt2go-objects.md)

# Multi Sensors
To control the multi sensors, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Set On
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set",
	"value": "on"
}
```

### Set Off
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set",
	"value": "off"
}
```

### Query Temperature
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "query",
	"value": "temperature"
}
```

### Query Humidity
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "query",
	"value": "humidity"
}
```

### Query Motion
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "query",
	"value": "motion"
}
```

### Query Smoke
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "query",
	"value": "smoke"
}
```

### Query Water
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "query",
	"value": "water"
}
```


## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### State Report
```json
{
	"type": "report",
	"priority_level": 2,
	"report_type":"report_type",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "on"
}
```

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "off"
}
```

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "state",
	"report": "repair_needed"
}
```

### Temperature Report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "temperature",
	"report": {
		"value:": 21.8,
		"unit": "C"
	}
}
```


### Humidity Report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "humidity",
	"report": {
		"value": 35.5,
		"unit": "%"
	}
}
```


### Motion Report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "motion",
	"report": "motion"
}
```


### Smoke Report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "smoke",
	"report": "smoke"
}
```


### Water Report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type": "report_type",
	"timestamp": 1567677956,
	"report_name": "water",
	"report": "water"
}
```

[Back](../mqtt2go-objects.md)