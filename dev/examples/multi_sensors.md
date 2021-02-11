[Back](../mqtt2go-objects.md)

# Multi Sensors
To control the multi sensors, a user have to follow the MQTT2GO convention. To ease up the setup, this page is dedicated to the command and report messages utilized for such communication.

## <a name="commands"></a>Commands
As specified by the MQTT2GO convention, the commands are utilized for device control. In the following subsection, we present the standardized way which is following the convention.

### Set On

```
<home_id>/<gw_id>/<device_id>/switch/in
```

```json
{
    "type": "set",
	"timestamp": 1567677926,
	"value": "on"
}
```

### Set Off

```
<home_id>/<gw_id>/<device_id>/switch/in
```

```json
{
    "type": "set",
	"timestamp": 1567677926,
	"value": "off"
}
```

```
<home_id>/<gw_id>/<device_id>/temperature/in
```

### Query Temperature
```json
{
    "type": "query",
	"timestamp": 1567677926,
	"value": "temperature"
}
```

```
<home_id>/<gw_id>/<device_id>/humidity/in
```

### Query Humidity
```json
{
	"type": "query",
	"timestamp": 1567677926,
	"value": "humidity"
}
```

```
<home_id>/<gw_id>/<device_id>/motion/in
```

### Query Motion
```json
{
	"type": "query",
	"timestamp": 1567677926,
	"value": "motion"
}
```

```
<home_id>/<gw_id>/<device_id>/smoke/in
```

### Query Smoke
```json
{
	"type": "query",
	"timestamp": 1567677926,
	"value": "smoke"
}
```

```
<home_id>/<gw_id>/<device_id>/water/in
```

### Query Water
```json
{
	"type": "query",
	"timestamp": 1567677926,
	"value": "water"
}
```


## <a name="reports"></a>Reports
Reports are utilized either as responses to the commands or to report periodic or critical event.

### State Report
```
<home_id>/<gw_id>/<device_id>/state/out
```
```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "on"
}
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "off"
}
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "repair_needed"
}
```

### Temperature Report

```
<home_id>/<gw_id>/<device_id>/temperature/out
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value:": 21.8,
		"unit": "C"
	}
}
```


### Humidity Report

```
<home_id>/<gw_id>/<device_id>/humidity/out
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": {
		"value": 35.5,
		"unit": "%"
	}
}
```


### Motion Report

```
<home_id>/<gw_id>/<device_id>/motion/out
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "motion"
}
```


### Smoke Report

```
<home_id>/<gw_id>/<device_id>/smoke/out
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "smoke"
}
```


### Water Report

```
<home_id>/<gw_id>/<device_id>/water/out
```

```json
{
	"type": "report_type",
	"priority_level": 2,
	"timestamp": 1567677956,
	"report": "water"
}
```

[Back](../mqtt2go-objects.md)