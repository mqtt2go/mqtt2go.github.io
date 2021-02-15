[Back](../mqtt2go-objects.md)

# Multi Sensors
To control the multi sensors, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

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

### Query Temperature
```json
{
	"timestamp": 1567677926,
	"value": "temperature"
}
```

### Query Humidity
```json
{
	"timestamp": 1567677926,
	"value": "humidity"
}
```

### Query Motion
```json
{
	"timestamp": 1567677926,
	"value": "motion"
}
```

### Query Smoke
```json
{
	"timestamp": 1567677926,
	"value": "smoke"
}
```

### Query Water
```json
{
	"timestamp": 1567677926,
	"value": "water"
}
```


## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### State Report
```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "on"
}
```

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "off"
}
```

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "repair_needed"
}
```

### Temperature Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value:": 21.8,
		"unit": "C"
	}
}
```


### Humidity Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value": 35.5,
		"unit": "%"
	}
}
```


### Motion Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "motion"
}
```


### Smoke Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "smoke"
}
```


### Water Report

```json
{
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "water"
}
```

[Back](../mqtt2go-objects.md)