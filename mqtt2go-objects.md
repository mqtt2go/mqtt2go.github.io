[Back](./index.md#data-structure)
# MQTT Objects
<p align="justify">
MQTT objects are describing all end devices that are connected to the MQTT2GO system. Their primary purpose is to either periodically report measurement results back to the SH-GW or to fulfill their role in the smart home.
</p>

## Topics Structure
<p align="justify">
The topic structure for MQTT objects is in line with the general structure described in 2.1.1. The topic structure is following.
</p>

```
<home_id>/<gateway_id>/<group_id>/<device_type>/<dev_id>
```

## MQTT Commands
<p align="justify">
The MQTT Commands described in this section are device specific and therefore they are arranged in tables for better understanding. The structure of the command is again compliant to the general structure from  2.1.2.
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
| Garage door | garage_door | set <br/> set_timer | Up, Down, Stop <br/> &lt;Timer&gt; |
| Smart Watering | smart_water | set <br/> set_timer | On, Off<br/> &lt;Timer&gt; |
| Lawnmowers | lawnmower | set <br/> setup | On, Off, Start, Stop <br/> &lt;Setup&gt; |
| Security Cameras | camera | set <br/> query | On, Off <br/> Stream |
| Doorbell | doorbell | set <br/> query | Disable, Enable <br/> Stream |
| Doorlock | doorlock | set <br/> setup | Lock, Unlock <br/> &lt;Setup&gt; |
| Blinds and Sunscreens | blinds | set <br/> set_timer | Up, Down, Stop <br/> &lt;Timer&gt; |
| Smart Sockets | socket | set <br/> set_timer <br/> query | On, Off <br/> &lt;Timer&gt; <br/> Consumption |
| Smart Plant Pots | plant_pot | setup <br/> set <br/> set_timer <br/> query | &lt;Setup&gt; <br/> WateringStart, WateringStop <br/> &lt;Timer&gt; <br/> Moisture, PH, WaterLevel |
| Motion Sensors | motion | set <br/> query | On, Off <br/> Motion |
| Temperature Sensors | temperature | set <br/> query | On, Off <br/> Temperature |
| Multi Sensors | multi_sensor | set <br/> query | On, Off <br/> Temperature, Motion, Smoke, Water |
| Smart Speakers | speaker | set | On, Off, Play, Shuffle, VolumeUp, VolumeDown, VolumeLevel |
| Smart TVs | tv | set <br/> set_channe <br/> set_volume_level | On, Off, VolumeUp, VolumeDown, Play, Pause <br/> &lt;Channel&gt; <br/> &lt;Volume&gt; |
| Thermostat | thermostat | query <br/> set_temperature <br/> setup | Temperature <br/> &lt;Temperature&gt; <br/> &lt;Setup&gt; |
| Vacuum Cleaners | vacuum | set <br/> setup | On, Off, SendToDock <br/> &lt;Setup&gt; |
| Flood / Water Sensor | flood | set | On, Off |
| Health Sensors | health | set <br/> query <br/> setup | On, Off <br/> Weight, Temperature, BMI, Pressure <br/> &lt;Setup&gt; |
| Washers & Dryers | washer_dryer | set <br/> set_timer <br/> setup | On, Off <br/> &lt;Timer&gt; <br/> &lt;Setup&gt; |
| Smart Spots / Lights | light | set <br/> set_color <br/> set_brigtness <br/> set_temperature | On, Off <br/> &lt;Color&gt; <br/> &lt;Brightness&gt; <br/> &lt;Temperature&gt; |
| Radiator Valve | radiator | set <br/> set_temperature <br/> setup | On, Off <br/> SetTemperature <br/> &lt;Setup&gt; |
| Solar Panels | solar | set <br/> query <br/> setup | On, Off <br/> CurrentPower <br/> &lt;Setup&gt; |
| Curtains | curtains | set <br/> setup | Up, Down, Stop <br/> &lt;Setup&gt; |
| Climate Control | climate | set <br/> setup <br/> set_timer | On, Off <br/> &lt;Setup&gt; <br/> &lt;Timer&gt; |
| Smoke Detector | smoke | set <br/> query | On, Off <br/> Smoke |
| Coffee Machines | coffee | set <br/> set_timer <br/> setup | On, Off <br/> &lt;Timer&gt; <br/> &lt;Setup&gt; |
| Voice Assistants | voice_assistant | set <br/> setup | On, Off <br/> &lt;Setup&gt; |
| Dishwasher | dishwasher | set <br/> set_timer <br/> setup | On, Off <br/> &lt;Timer&gt; <br/> &lt;Setup&gt; |
| Keyfob & Remotes | keyfob_remote | set <br/> set_button <br/> setup | On, Off <br/> &lt;ButtonPress&gt; <br/> &lt;Setup&gt; |
| Ovens | oven | set <br/> set_timer <br/> setup | On, Off <br/> &lt;Timer&gt; <br/> &lt;Setup&gt; |
| Door / Window sensors | door | set <br/> query | On, Off <br/> State |
| Weather Stations | weather | set <br/> setup <br/> query <br/> | On, Off <br/> &lt;Setup&gt; <br/> Temperature, CurrentWeather, Humidity, Wind, UV, Forecast |
| Alarm | alarm | set <br/> <br/> query <br/> setup | On, Off, SirenOn, SirenOff, SirenWalkin, SirenWalkOut, Arm, Disarm <br/> State <br/> &lt;Setup&gt; |
| Utility meter | utility_meter | set <br/> setup <br/> query <br/> <br/>  <br/>  <br/> | On, Off <br/> &lt;Setup&gt; <br/> Voltage, Frequency, Tarif, CurrentConsumption, State, WeeklyConsumption, MonthlyConsumption |

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
	"command_type":"setColor",
	"value": {
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
| Garage door | garage_door | state | Closed, Open, Stopped, RepairNeeded |
| Smart Watering | smart_water | state | OutOfWater |
| Lawnmowers | lawnmower | state | Mowing, BatteryLow, BatteryOk, RepairNeeded |
| Security Cameras | camera | stream <br/> state | &lt;Stream_URL&gt; <br/> StreamFailed |
| Doorbell | doorbell | stream <br/> state | &lt;Stream_URL&gt; <br/> Ringing, StreamFailed |
| Doorlock | doorlock | state | Locked, Unlocked, LockBreach, LockJammed, LockFailed, UnlockFailed |
| Blinds and Sunscreens | blinds | state | Closed, Open, Stopped, RepairNeeded |
| Smart Sockets | socket | state <br/> consumption | On, Off <br/> &lt;Consumption&gt; |
| Smart Plant Pots | plant_pot | state | OutOfWater |
| Motion Sensors | motion | state | MotionPresent |
| Temperature Sensors | temperature | temperature | &lt;Temperature&gt; |
| Multi Sensors | multi_sensor | state <br/> temperature <br/> motion <br/> smoke <br/> water | On, Off, RepairNeeded  <br/> &lt;Temperature&gt; <br/> Motion <br/> Smoke <br/> Water |
| Smart Speakers | speaker | state | On, Off, NoMedia, Playing, StreamingError, RepairNeeded |
| Smart TVs | tv | state <br/> channel_number <br/> volume_level | On, Off, RepairNeeded <br/> &lt;ChannelNumber&gt; <br/> &lt;VolumeLevel&gt; |
| Thermostat | thermostat | current_temperature <br/> set_temperature <br/> state | &lt;CurrTemperature&gt; <br/> &lt;SetTemperature&gt; <br/> &lt;Mode&gt; |
| Vacuum Cleaners | vacuum | state | On, Off, Vacuuming, BatteryLow, Stuck, RepairNeeded |
| Flood / Water Sensor | flood | state | Flood |
| Health Sensors | health | state <br/> weight <br/> temperature <br/> bmi <br/> pressure | On, Off, SensorError <br/> &lt;Weigh&gt; <br/> &lt;Temperature&gt; <br/> &lt;BMI&gt; <br/> &lt;Pressure&gt; |
| Washers & Dryers | washer_dryer | state <br/> <br/> timer | On, Off, InProgress, WaterError, RepairNeeded <br/> &lt;Timer&gt; |
| Smart Spots / Lights | light | state <br/> color <br/> brightness <br/> temperature | On, Off <br/> &lt;Color&gt; <br/> &lt;Brightness&gt; <br/> &lt;Temperature&gt; |
| Radiator Valve | radiator | state <br/> current_temperature <br/> set_temperature | On, Off <br/> &lt;CurrTemperature&gt; <br/> &lt;SetTemperature&gt; |
| Solar Panels | solar | state <br/> power | On, Off, PanelError, BatteryError <br/> &lt;CurrPower&gt; |
| Wall Switches / Built-in Switches | switch | state <br/> timer | On, Off, Mode <br/> &lt;SetTimer&gt; |
| Curtains | curtains | state | Closed, Open, Stopped, Stucked |
| Climate Control | climate | state <br/> timer <br/> humidity | On, Off <br/> &lt;SetTimer&gt; <br/> &lt;Humidity&gt; |
| Smoke Detector | smoke | state | SmokeAlarm |
| Coffee Machines | coffee | state <br/> <br/> timer | On, Off, WaterLow, WaterOK, CoffeeLow, CoffeeOK, MilkLow, MilkOK, CleaningNeeded,
RepairNeeded <br/> &lt;SetTimer&gt; |
| Voice Assistants | voice_assistant | state <br/> setup | On, Off <br/> &lt;Setup&gt; |
| Dishwasher | dishwasher | state <br/> <br/> timer | On, Off, InProgress, WaterError, RepairNeeded <br/> &lt;SetTimer&gt; |
| Keyfob & Remotes | keyfob_remote | state <br/> mode | On, Off <br/> &lt;Mode&gt; |
| Ovens | oven | state <br/> timer <br/> mode <br/> temperature <br/> | On, Off, LightOn, LightOff <br/> &lt;SetTimer&gt; <br/> &lt;Mode&gt; <br/> &lt;Temperature&gt; |
| Door / Window sensors | door | state | Open, Closed |
| Weather Stations | weather | state <br/> temperature <br/> current_weather <br/> humidity <br/> wind <br/> uv <br/> forecast | On, Off <br/> &lt;Temperature&gt; <br/> &lt;CurrentWeather&gt; <br/> &lt;Humidity&gt; <br/> &lt;Wind&gt; <br/> &lt;UV&gt; <br/> &lt;Forecast&gt; |
| Alarm | alarm | state | AlarmButtonPressed, AlarmOffButtonPressed, ArcAlert, ArcAlertArmStatus, ArcAlertDeprovision, ArcAlertRevision, ArcError, ArcTestResult, ArmStatusChange, GlassBreakage, PanicButtonPressed, TamperCleared, TamperDetected |
| Utility meter | utility_meter | state <br/> value | On, Off <br/> UtilityValue |

### MQTT Reports Examples
<p align="justify">
As for the commands, here we present two examples of the reports:
</p>

#### Simple Report Example
```json
{
	"type": "report",
	"priority_level":1,
	"timestamp":1567677946,
	"report_type":"OnReport",
	"report":"on"
}
```

#### Complex Report Example
```json
{
	"type": "report",
	"priority_level": 1,
	"timestamp":1567677956,
	"report_type":"ColorReport",
	"report": {
				"h": 100, 
				"s": 100,
				"b": 50
			  }
}
```

[Back](./index.md#data-structure)
