[Back](./index.md#data-structure)
# MQTT Objects
<p align="justify">
MQTT objects are describing all end devices that are connected to the MQTT2GO system. Their primary purpose is to either periodically report measurement results back to the SH-GW or to be controlled by the users and controllers to fulfill their role in the smart home.
</p>

## Topics Structure
<p align="justify">
The topic structure for MQTT objects is in line with the general structure described in <a href="./mqtt2go-commands#mqtt_topics">MQTT Topics</a>. The topic structure is following.
</p>

```
<home_id>/<gateway_id>/<dev_id>/<entity>/<msg_direction>
```

Furthermore, as the MQTT standard allows users to define message retention, it is advised to turn this feature on for the MQTT2GO standard topics. This way, it can be made sure the devices will always know the last set state and behave accordingly.

## <a name="object-commands"></a>MQTT Commands
<p align="justify">
The MQTT Commands described in this section are device specific and therefore they are arranged in tables for better understanding. The structure of the command is again compliant to the general structure from  <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>.
</p>

### Description of command types
<p align="justify">
To distinguish between the MQTT2GO commands that are going to the device, the <strong>msg_direction</strong> part of the topic is utilized. For all the commands, the <strong>msg_direction</strong> has to be set to <strong>in</strong>. Furthermore, to distinguish between different type of messages, the <strong>type</strong> is utilized. For the commands, it is mostly <strong>set</strong> type. If the type is <strong>set</strong>, the <strong>value</strong> contains the value that is going to be set. If the type is from the <strong>query</strong> "family", the <strong>value</strong> is ommited. If more complex setup is needed, a JSON structure will be sent inside the <strong>value</strong>.
</p>

### Table with Commands

| Name                  | Device type                                          | Type | Command                                                                                                                          |
|-----------------------|------------------------------------------------------|------|----------------------------------------------------------------------------------------------------------------------------------|
| Garage door           | garage_door                                          | set | up, down, stop <br/> &lt;timer&gt;                                                                                               |
| Smart Watering        | smart_water                                          | set  | on, off<br/> &lt;timer&gt;                                                                                                       |
| Lawnmowers            | lawnmower                                            | set  | on, off, start, stop <br/> &lt;setup&gt;                                                                                         |
| Security Cameras      | camera                                               | set <br> stream  | on, off <br/> stream                                                                                                             |
| Doorbell              | doorbell                                             | set <br> stream | disable, enable <br/> stream                                                                                                     |
| Doorlock              | doorlock                                             | set  | lock, unlock <br/> &lt;setup&gt;                                                                                                 |
| Blinds and Sunscreens | [blinds](./examples/blinds.md#commands)              | set  | up, down, stop <br/> &lt;timer&gt;                                                                                               |
| Smart Sockets         | [socket](./examples/sockets.md#commands)             | set  | on, off <br/> &lt;timer&gt; <br/> consumption, current, voltage, power                                                           |
| Smart Plant Pots      | plant_pot                                            | set  | &lt;setup&gt; <br/> watering_start, watering_stop <br/> &lt;timer&gt; <br/> moisture, ph, water_level                            |
| Motion Sensors        | motion                                               | set  | on, off <br/> motion                                                                                                             |
| Temperature Sensors   | temperature                                          | set  | on, off <br/> temperature                                                                                                        |
| Multi Sensors         | [multi_sensor](./examples/multi_sensors.md#commands) | set  | on, off <br/> temperature, humidity, motion, smoke, water                                                                        |
| Smart Speakers        | speaker                                              | set  | on, off, play, shuffle, volume_up, volume_down, volume_level                                                                     |
| Smart TVs             | tv                                                   | set  | on, off, volume_up, volume_down, play, pause <br/> &lt;channel&gt; <br/> &lt;volume&gt;                                          |
| Thermostat            | thermostat                                           | set  | temperature <br/> &lt;temperature&gt; <br/> &lt;setup&gt;                                                                        |
| Vacuum Cleaners       | vacuum                                               | set  | on, off, send_to_dock <br/> &lt;setup&gt;                                                                                        |
| Flood / Water Sensor  | flood                                                | set  | on, off                                                                                                                          |
| Health Sensors        | health                                               | set  | on, off <br/> weight, temperature, bmi, pressure <br/> &lt;setup&gt;                                                             |
| Washers & Dryers      | washer_dryer                                         | set  | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt;                                                                                  |
| Smart Spots / Lights  | [light](./examples/light.md#commands)                                                | set <br> color <br> brightness <br> color_temp | on, off <br/> &lt;color&gt; <br/> &lt;brightness&gt; <br/> &lt;temperature&gt;                                                   |
| Radiator Valve        | radiator                                             | set  | on, off <br/> set_temperature <br/> &lt;setup&gt;                                                                                |
| Solar Panels          | solar                                                | set  | on, off <br/> current_power <br/> &lt;setup&gt;                                                                                  |
| Curtains              | curtains                                             | set  | up, down, stop <br/> &lt;setup&gt;                                                                                               |
| Climate Control       | climate                                              | set  | on, off <br/> &lt;setup&gt; <br/> &lt;timer&gt;                                                                                  |
| Smoke Detector        | smoke                                                | set  | on, off <br/> smoke                                                                                                              |
| Coffee Machines       | coffee                                               | set  | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt;                                                                                  |
| Voice Assistants      | voice_assistant                                      | set  | on, off <br/> &lt;setup&gt;                                                                                                      |
| Dishwasher            | dishwasher                                           | set  | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt;                                                                                  |
| Keyfob & Remotes      | keyfob_remote                                        | set  | on, off <br/> &lt;button_press&gt; <br/> &lt;setup&gt;                                                                           |
| Ovens                 | oven                                                 | set  | on, off <br/> &lt;timer&gt; <br/> &lt;setup&gt;                                                                                  |
| Door / Window sensors | door                                                 | set  | on, off <br/> state                                                                                                              |
| Weather Stations      | weather                                              | set  | on, off <br/> &lt;setup&gt; <br/> temperature, current_weather, humidity, wind, uv, forecast                                     |
| Alarm                 | alarm                                                | set  | on, off, siren_on, siren_off, siren_walk_in, siren_walk_out, arm, disarm <br/> state <br/> &lt;setup&gt;                         |
| Utility meter         | utility_meter                                        | set  | on, off <br/> &lt;setup&gt; <br/> voltage, frequency, tarif, current_consumption, state, weekly_consumption, monthly_consumption |

#### MQTT Device Removal Process
<p align="justify">
To be able to remove a device from the MQTT2GO system, a remove topic has to be used. The command structure is presented below.
</p>

```
<home_id>/<gw_id>/<device_id>/remove/in
```

```json
{
    "type": "control",
    "timestamp": 1567677926,
    "value": "true" 
}
```

### MQTT Commands Examples
<p align="justify">
To provide some examples of the MQTT Commands usage, we provide simple and complex structure examples.
</p>

#### Simple Command Example

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

#### Complex Command Example

```
<home_id>/<gw_id>/<device_id>/color/in
```

```json
{
    "type": "color",
    "timestamp":1567677926,
    "value": {
        "r": 100, 
        "g": 100,
        "b": 50
	}
}
```

## MQTT Reports
<p align="justify">
The MQTT Reports are used for the periodic (with type of periodic_report) and forced reports (type of command_response). Their main purpose is therefore to report the measured values or action results back to the controlling app.
</p>

### Table with Reports

| Name                              | Device type                                         | Report                                                                                                                                                                                                                                           |
|-----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Garage door                       | garage_door                                         | closed, open, stopped, repair_needed                                                                                                                                                                                                             |
| Smart Watering                    | smart_water                                         | out_of_water                                                                                                                                                                                                                                     |
| Lawnmowers                        | lawnmower                                           | mowing, battery_low, battery_ok, repair_needed                                                                                                                                                                                                   |
| Security Cameras                  | camera                                              | &lt;stream_url&gt; <br/> stream_failed                                                                                                                                                                                                           |
| Doorbell                          | doorbell                                            | &lt;stream_url&gt; <br/> ringing, stream_failed                                                                                                                                                                                                  |
| Doorlock                          | doorlock                                            | locked, unlocked, lock_breach, lock_jammed, lock_failed, unlock_failed                                                                                                                                                                           |
| Blinds and Sunscreens             | [blinds](./examples/blinds.md#reports)              | closed, open, stopped, repair_needed                                                                                                                                                                                                             |
| Smart Sockets                     | [socket](./examples/sockets.md#reports)             | on, off <br/> &lt;consumption&gt; <br/> &lt;current&gt; <br/> &lt;voltage&gt; <br/> &lt;power&gt;                                                                                                                                                |
| Smart Plant Pots                  | plant_pot                                           | out_of_water                                                                                                                                                                                                                                     |
| Motion Sensors                    | motion                                              | motion_present                                                                                                                                                                                                                                   |
| Temperature Sensors               | temperature                                         | &lt;temperature&gt;                                                                                                                                                                                                                              |
| Multi Sensors                     | [multi_sensor](./examples/multi_sensors.md#reports) | on, off, repair_needed  <br/> &lt;temperature&gt; <br/> &lt;humidity&gt; <br/> motion <br/> smoke <br/> water                                                                                                                                    |
| Smart Speakers                    | speaker                                             | on, off, no_media, playing, streaming_error, repair_needed                                                                                                                                                                                       |
| Smart TVs                         | tv                                                  | on, off, repair_needed <br/> &lt;channel_number&gt; <br/> &lt;volume_level&gt;                                                                                                                                                                   |
| Thermostat                        | thermostat                                          | &lt;curr_temperature&gt; <br/> &lt;set_temperature&gt; <br/> &lt;mode&gt;                                                                                                                                                                        |
| Vacuum Cleaners                   | vacuum                                              | on, off, vacuuming, battery_low, stuck, repair_needed                                                                                                                                                                                            |
| Flood / Water Sensor              | flood                                               | flood                                                                                                                                                                                                                                            |
| Health Sensors                    | health                                              | on, off, sensor_error <br/> &lt;weigh&gt; <br/> &lt;temperature&gt; <br/> &lt;bmi&gt; <br/> &lt;pressure&gt;                                                                                                                                     |
| Washers & Dryers                  | washer_dryer                                        | on, off, in_progress, water_error, repair_needed <br/> &lt;timer&gt;                                                                                                                                                                             |
| Smart Spots / Lights              | [light](./examples/lights.md#reports)               | on, off <br/> &lt;color&gt; <br/> &lt;brightness&gt; <br/> &lt;temperature&gt;                                                                                                                                                                   |
| Radiator Valve                    | radiator                                            | on, off <br/> &lt;curr_temperature&gt; <br/> &lt;set_temperature&gt;                                                                                                                                                                             |
| Solar Panels                      | solar                                               | on, off, panel_error, battery_error <br/> &lt;curr_power&gt;                                                                                                                                                                                     |
| Wall Switches / Built-in Switches | switch                                              | on, off, mode <br/> &lt;set_timer&gt;                                                                                                                                                                                                            |
| Curtains                          | curtains                                            | closed, open, stopped, stuck                                                                                                                                                                                                                     |
| Climate Control                   | climate                                             | on, off <br/> &lt;set_timer&gt; <br/> &lt;humidity&gt;                                                                                                                                                                                           |
| Smoke Detector                    | smoke                                               | smoke_alarm                                                                                                                                                                                                                                      |
| Coffee Machines                   | coffee                                              | on, off, water_low, water_ok, coffee_low, coffee_ok, milk_low, milk_ok, cleaning_needed, repair_needed <br/> &lt;set_timer&gt;                                                                                                                   |
| Voice Assistants                  | voice_assistant                                     | on, off <br/> &lt;setup&gt;                                                                                                                                                                                                                      |
| Dishwasher                        | dishwasher                                          | on, off, in_progress, water_error, repair_needed <br/> &lt;set_timer&gt;                                                                                                                                                                         |
| Keyfob & Remotes                  | keyfob_remote                                       | on, off <br/> &lt;mode&gt;                                                                                                                                                                                                                       |
| Ovens                             | oven                                                | on, off, light_on, light_off <br/> &lt;set_timer&gt; <br/> &lt;mode&gt; <br/> &lt;temperature&gt;                                                                                                                                                |
| Door / Window sensors             | door                                                | open, closed                                                                                                                                                                                                                                     |
| Weather Stations                  | weather                                             | on, off <br/> &lt;temperature&gt; <br/> &lt;current_weather&gt; <br/> &lt;humidity&gt; <br/> &lt;wind&gt; <br/> &lt;uv&gt; <br/> &lt;forecast&gt;                                                                                                |
| Alarm                             | alarm                                               | alarm_button_pressed, alarm_off_button_pressed, arc_alert, arc_alert_arm_status, arc_alert_deprovision, arc_alert_revision, arc_error, arc_test_result, arm_status_change, glass_breakage, panic_button_pressed, tamper_cleared, tamper_detected |
| Utility meter                     | utility_meter                                       | on, off <br/> utility_value                                                                                                                                                                                                                      |

### MQTT Reports Examples
<p align="justify">
To provide complete example of the command - report message structure. Below we provide the example structure for simple and complex reports.
</p>

#### Simple Report Example

```
<home_id>/<gw_id>/<device_id>/switch/out
```

```json
{
    "priority_level":2,
    "timestamp":1567677946,
    "type":"command_response",
    "report":"on"
}
```

#### Complex Report Example

```
<home_id>/<gw_id>/<device_id>/color/out
```

```json
{
    "priority_level": 2,
    "timestamp":1567677956,
    "type":"command_response",
    "report": {
        "unit": "hsb",
        "h": 100, 
        "s": 100,
        "b": 50
    }
}
```

### MQTT2GO units
In this section, we provide a table with all currently utilized units in the MQTT2GO messages. If the unit needs to be specified for selected command/report (meaning the command/report is not simple on,off,etc.), then the value will be specified inside the *value* field. The utilized unit types are following:


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
		<td>Garage Doors<br/>Smart Watering<br/>Blinds and Sunscreens<br/>Smart Sockets<br/>Smart Plant Pots<br/>Washers & Dryers<br/>Climate Control<br/>Coffee Machines<br/>Dishwashers<br/>Ovens</td><td>Timer</td><td>hh:mm:ss</td>
	</tr>
	<tr>
		<td>Smart Plant Pots<br/> Weather Stations</td><td>Humidity<br/>PH</td><td>%<br/>0-14</td>
	</tr>
	<tr>
		<td>Health Sensors</td><td>Weight<br/>BMI<br/>Pressure</td><td>kg, lb<br/>kg/m2<br/>mmHg</td>
	</tr>
	<tr>
		<td>Smart Spots / Lights</td><td>Brightness<br/>Color<br/>Temperature</td><td>%<br/>hsb, rgb, rgbwaf, xy</td>
	</tr>
	<tr>
		<td>Weather Stations</td><td>Wind<br/>UV</td><td>m/s, km/h<br/>mW/cm2, mJ/cm2</td>
	</tr>
	<tr>
		<td>Smart TVs</td><td>Volume</td><td>%</td>
	</tr>
</table>

The units specified above are for currently supported devices. If a new type of device will be added, the unit structure should follow the MQTT2GO standard.


#### MQTT2GO About Topic Example

##### About Command
```
<home_id>/<gw_id>/<device_id>/about/in
```

```json
{
    "priority_level": 2,
    "timestamp":1567677956,
    "type":"query"
}
```

##### About Response

```
<home_id>/<gw_id>/<device_id>/about/in
```

```json
{
    "priority_level": 2,
    "timestamp":1567677956,
    "type":"command_response",
    "value": {
         "device_id": "device_id_value",
         "topics": ["topic_name_1", ...],
         "status": "status_value"
    }
}
```

[Back](./index.md#data-structure)
