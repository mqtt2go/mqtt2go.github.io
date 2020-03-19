[Back](./index.md#data-structure)
# MQTT Objects
<p align="justify">
MQTT objects are describing all end devices that are connected to the MQTT2GO system. Their primary purpose is to either periodically report measurement results back to the SH-GW or to fulfill their role in the smart home.
</p>

## Topics Structure
<p align="justify">
The topic structure for MQTT objects is in line with the general structure described in <a href="./mqtt2go-commands#mqtt_topics">MQTT Topics</a>. The topic structure is following.
</p>

```
<home_id>/<gateway_id>/<group_id>/<device_type>/<dev_id>
```

## <a name="object-commands"></a>MQTT Commands
<p align="justify">
The MQTT Commands described in this section are device specific and therefore they are arranged in tables for better understanding. The structure of the command is again compliant to the general structure from  <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>.
</p>

### Description of command types
<p align="justify">
The command types used in the MQTT Commands are describing the targeted functionality of the whole command. The command types are:
</p>

* Set commands, which can be further specified by underscore to match a certain command. I.e., set_temperature.
* Query commands, which are used to query information from the end devices. They are used to force the readout out of the periodic report time cycle.

### Table with Commands

| Name | Device type | Command Type | Command |
| --- | --- | --- | --- |
| Garage door | garage_door | set <br/> set_timer | up, down, stop <br/> &lt;timer&gt; |
| Smart Watering | smart_water | set <br/> set_timer | on, off<br/> &lt;timer&gt; |
| Lawnmowers | lawnmower | set <br/> setup | on, off, start, stop <br/> &lt;setup&gt; |
| Security Cameras | camera | set <br/> query | on, off <br/> stream |
| Doorbell | doorbell | set <br/> query | disable, enable <br/> stream |
| Doorlock | doorlock | set <br/> setup | lock, unlock <br/> &lt;setup&gt; |
| Blinds and Sunscreens | [blinds](./examples/blinds.md#commands) | set <br/> set_timer | up, down, stop <br/> &lt;timer&gt; |
| Smart Sockets | socket | set <br/> set_timer <br/> query | on, off <br/> &lt;timer&gt; <br/> consumption |
| Smart Plant Pots | plant_pot | setup <br/> set <br/> set_timer <br/> query | &lt;setup&gt; <br/> watering_start, watering_stop <br/> &lt;timer&gt; <br/> moisture, ph, water_level |
| Motion Sensors | motion | set <br/> query | on, off <br/> motion |
| Temperature Sensors | temperature | set <br/> query | on, off <br/> temperature |
| Multi Sensors | multi_sensor | set <br/> query | on, off <br/> temperature, motion, smoke, water |
| Smart Speakers | speaker | set | on, off, play, shuffle, volume_up, volume_down, volume_level |
| Smart TVs | tv | set <br/> set_channel <br/> set_volume_level | on, off, volume_up, volume_down, play, pause <br/> &lt;channel&gt; <br/> &lt;volume&gt; |
| Thermostat | thermostat | query <br/> set_temperature <br/> setup | temperature <br/> &lt;temperature&gt; <br/> &lt;setup&gt; |
| Vacuum Cleaners | vacuum | set <br/> setup | on, off, send_to_dock <br/> &lt;setup&gt; |
| Flood / Water Sensor | flood | set | on, off |
| Health Sensors | health | set <br/> query <br/> setup | on, off <br/> weight, temperature, bmi, pressure <br/> &lt;setup&gt; |
| Washers & Dryers | washer_dryer | set <br/> set_timer <br/> setup | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt; |
| Smart Spots / Lights | light | set <br/> set_color <br/> set_brightness <br/> set_temperature | on, off <br/> &lt;color&gt; <br/> &lt;brightness&gt; <br/> &lt;temperature&gt; |
| Radiator Valve | radiator | set <br/> set_temperature <br/> setup | on, off <br/> set_temperature <br/> &lt;setup&gt; |
| Solar Panels | solar | set <br/> query <br/> setup | on, off <br/> current_power <br/> &lt;setup&gt; |
| Curtains | curtains | set <br/> setup | up, down, stop <br/> &lt;setup&gt; |
| Climate Control | climate | set <br/> setup <br/> set_timer | on, off <br/> &lt;setup&gt; <br/> &lt;timer&gt; |
| Smoke Detector | smoke | set <br/> query | on, off <br/> smoke |
| Coffee Machines | coffee | set <br/> set_timer <br/> setup | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt; |
| Voice Assistants | voice_assistant | set <br/> setup | on, off <br/> &lt;setup&gt; |
| Dishwasher | dishwasher | set <br/> set_timer <br/> setup | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt; |
| Keyfob & Remotes | keyfob_remote | set <br/> set_button <br/> setup | on, off <br/> &lt;button_press&gt; <br/> &lt;setup&gt; |
| Ovens | oven | set <br/> set_timer <br/> setup | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt; |
| Door / Window sensors | door | set <br/> query | on, off <br/> state |
| Weather Stations | weather | set <br/> setup <br/> query <br/> | on, off <br/> &lt;setup&gt; <br/> temperature, current_weather, humidity, wind, uv, forecast |
| Alarm | alarm | set <br/> <br/> query <br/> setup | on, off, siren_on, siren_off, siren_walk_in, siren_walk_out, arm, disarm <br/> state <br/> &lt;setup&gt; |
| Utility meter | utility_meter | set <br/> setup <br/> query <br/> <br/>  <br/>  <br/> | on, off <br/> &lt;setup&gt; <br/> voltage, frequency, tarif, current_consumption, state, weekly_consumption, monthly_consumption |

### MQTT Commands Examples
<p align="justify">
To provide some examples of the MQTT Commands usage, we provide simple and complex examples.
</p>

#### Simple Command Example
```json
{
	"type": "command",
	"timestamp": 1567677926,
	"command_type": "set",
	"value": "on" 
}
```

#### Complex Command Example
```json
{
	"type": "command",
	"timestamp":1567677926,
	"command_type":"set_color",
	"value": {
		"unit": "hsb",
		"h": 100, 
		"s": 100,
		"b": 50
	}
}
```

## MQTT Reports
<p align="justify">
The MQTT Reports are used for the periodic and forced reports (replies). Their main purpose is therefore to report the measured values or action results back to the controlling app.
</p>

### Description of Report Types
<p align="justify">
The report types are used to distinguish between different reports based on their purpose. They are further divided into:
</p>

* __Status__, which is reporting the current status of the device,
* __Command response__ used to immediately report after the command request,
* __Periodic report__ used to periodically publish current data/state into subscribed topics,
* __Error__ used to inform about critical states or malfunctions.

### Table with Reports

| Name | Device type | Report name | Report |
| --- | --- | --- | --- |
| Garage door | garage_door | state | closed, open, stopped, repair_needed |
| Smart Watering | smart_water | state | out_of_water |
| Lawnmowers | lawnmower | state | mowing, battery_low, battery_ok, repair_needed |
| Security Cameras | camera | stream <br/> state | &lt;stream_url&gt; <br/> stream_failed |
| Doorbell | doorbell | stream <br/> state | &lt;stream_url&gt; <br/> ringing, stream_failed |
| Doorlock | doorlock | state | locked, unlocked, lock_breach, lock_jammed, lock_failed, unlock_failed |
| Blinds and Sunscreens | [blinds](./examples/blinds.md#reports) | state | closed, open, stopped, repair_needed |
| Smart Sockets | socket | state <br/> consumption | on, off <br/> &lt;consumption&gt; |
| Smart Plant Pots | plant_pot | state | out_of_water |
| Motion Sensors | motion | state | motion_present |
| Temperature Sensors | temperature | temperature | &lt;temperature&gt; |
| Multi Sensors | multi_sensor | state <br/> temperature <br/> motion <br/> smoke <br/> water | on, off, repair_needed  <br/> &lt;temperature&gt; <br/> motion <br/> smoke <br/> water |
| Smart Speakers | speaker | state | on, off, no_media, playing, streaming_error, repair_needed |
| Smart TVs | tv | state <br/> channel_number <br/> volume_level | on, off, repair_needed <br/> &lt;channel_number&gt; <br/> &lt;volume_level&gt; |
| Thermostat | thermostat | current_temperature <br/> set_temperature <br/> state | &lt;curr_temperature&gt; <br/> &lt;set_temperature&gt; <br/> &lt;mode&gt; |
| Vacuum Cleaners | vacuum | state | on, off, vacuuming, battery_low, stuck, repair_needed |
| Flood / Water Sensor | flood | state | flood |
| Health Sensors | health | state <br/> weight <br/> temperature <br/> bmi <br/> pressure | on, off, sensor_error <br/> &lt;weigh&gt; <br/> &lt;temperature&gt; <br/> &lt;bmi&gt; <br/> &lt;pressure&gt; |
| Washers & Dryers | washer_dryer | state <br/> <br/> timer | on, off, in_progress, water_error, repair_needed <br/> &lt;timer&gt; |
| Smart Spots / Lights | light | state <br/> color <br/> brightness <br/> temperature | on, off <br/> &lt;color&gt; <br/> &lt;brightness&gt; <br/> &lt;temperature&gt; |
| Radiator Valve | radiator | state <br/> current_temperature <br/> set_temperature | on, off <br/> &lt;curr_temperature&gt; <br/> &lt;set_temperature&gt; |
| Solar Panels | solar | state <br/> power | on, off, panel_error, battery_error <br/> &lt;curr_power&gt; |
| Wall Switches / Built-in Switches | switch | state <br/> timer | on, off, mode <br/> &lt;set_timer&gt; |
| Curtains | curtains | state | closed, open, stopped, stuck |
| Climate Control | climate | state <br/> timer <br/> humidity | on, off <br/> &lt;set_timer&gt; <br/> &lt;humidity&gt; |
| Smoke Detector | smoke | state | smoke_alarm |
| Coffee Machines | coffee | state <br/> <br/> timer | on, off, water_low, water_ok, coffee_low, coffee_ok, milk_low, milk_ok, cleaning_needed,
repair_needed <br/> &lt;set_timer&gt; |
| Voice Assistants | voice_assistant | state <br/> setup | on, off <br/> &lt;setup&gt; |
| Dishwasher | dishwasher | state <br/> <br/> timer | on, off, in_progress, water_error, repair_needed <br/> &lt;set_timer&gt; |
| Keyfob & Remotes | keyfob_remote | state <br/> mode | on, off <br/> &lt;mode&gt; |
| Ovens | oven | state <br/> timer <br/> mode <br/> temperature <br/> | on, off, light_on, light_off <br/> &lt;set_timer&gt; <br/> &lt;mode&gt; <br/> &lt;temperature&gt; |
| Door / Window sensors | door | state | open, closed |
| Weather Stations | weather | state <br/> temperature <br/> current_weather <br/> humidity <br/> wind <br/> uv <br/> forecast | on, off <br/> &lt;temperature&gt; <br/> &lt;current_weather&gt; <br/> &lt;humidity&gt; <br/> &lt;wind&gt; <br/> &lt;uv&gt; <br/> &lt;forecast&gt; |
| Alarm | alarm | state | alarm_button_pressed, alarm_off_button_pressed, arc_alert, arc_alert_arm_status, arc_alert_deprovision, arc_alert_revision, arc_error, arc_test_result, arm_status_change, glass_breakage, panic_button_pressed, tamper_cleared, tamper_detected |
| Utility meter | utility_meter | state <br/> value | on, off <br/> utility_value |

### MQTT Reports Examples
<p align="justify">
As for the commands, here we present two examples of the reports:
</p>

#### Simple Report Example
```json
{
	"type": "report",
	"priority_level":2,
	"report_type":"command_response",
	"timestamp":1567677946,
	"report_name":"state",
	"report":"on"
}
```

#### Complex Report Example
```json
{
	"type": "report",
	"priority_level": 2,
	"report_type":"command_response",
	"timestamp":1567677956,
	"report_name":"color",
	"report": {
		"unit": "hsb",
		"h": 100, 
		"s": 100,
		"b": 50
	}
}
```

### MQTT2GO units
In this section, we provide a table with all units needed by the MQTT2GO messages. If the unit needs to be specified for selected command/report (meaning the command/report is not simple on,off,etc.), then the value will be specfied inside the value field. The units are following:


<table style="width:100%">
	<tr>
		<th style="width:40%">Device type</th><th style="width:40%">Data type</th><th style="width:20%">Unit</th>
	</tr>
	<tr>
		<td>Smart Sockets<br/> Solar Panels</td><td>Power</td><td>W</td>
	</tr>
	<tr>
		<td>Multi Sensors<br/>Thermostats<br/>Radiator Valves<br/>Health Sensors<br/>Climate Control<br/>Ovens<br/>Weather Stations</td><td>Temperature</td><td>C, F, K</td>
	</tr>
	<tr>
		<td>Garage Doors<br/>Smart Watering<br/>Blinds and Sunscreens<br/>Smart Sockets<br/>Smart Plant Pots<br/>Washers & Dryers<br/>Climate Control<br/>Coffee Machines<br/>Dishwashers<br/>Ovens</td><td>Timer</td><td>s</td>
	</tr>
	<tr>
		<td>Smart Plant Pots<br/> Weather Stations</td><td>Humidity<br/>PH</td><td>%<br/>0-14</td>
	</tr>
	<tr>
		<td>Health Sensors</td><td>Weight<br/>BMI<br/>Pressure</td><td>kg, lb<br/>kg/m2<br/>mmHg</td>
	</tr>
	<tr>
		<td>Smart Spots / Lights</td><td>Brightness<br/>Color</td><td>%<br/>hsb, rgb</td>
	</tr>
	<tr>
		<td>Weather Stations</td><td>Wind<br/>UV</td><td>m/s, km/h<br/>mW/cm2, mJ/cm2</td>
	</tr>
	<tr>
		<td>Smart TVs</td><td>Volume</td><td>%</td>
	</tr>
	<tr>
		<td>Security Cameras<br/>Doorbells</td><td>Stream</td><td>URL address</td>
	</tr>
</table>

The units specified above are for currently supported devices. If a new type of device will be added, the unit structure should follow the MQTT2GO standard.

#### MQTT2GO units example
Here we provide sample command and report to depict the correct usage of units:

##### MQTT2GO unit command
```json
{
	"type": "command",
	"timestamp":1567677926,
	"command_type":"set_color",
	"value": {
		"unit": "hsb",
		"h": 100, 
		"s": 100,
		"b": 50
	}
}
```

##### MQTT2GO unit report

```json
{
	"type": "report",
	"priority_level": 2,
	"report_type":"command_response",
	"timestamp":1567677956,
	"report_name":"color",
	"report": {
		"unit": "hsb",
		"h": 100, 
		"s": 100,
		"b": 50
	}
}
```

[Back](./index.md#data-structure)
