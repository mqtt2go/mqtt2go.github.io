[Back](./index.md)

# Changelog
* Improved structure of the MQTT2GO topics
	* Each entity has in and out direction,
	* Each command must be confirmed by message in entity out topic,
	* New _Get entities_ command and report are added to MQTT2GO controllers,
	* Wildcard explanation was added.

* Adding procedure has been updated:
	* New home prefix procedure was designed to obtain home_id and gateway_id,
	* Adding procedure messages were reordered,
	Adding procedure of non-complaint devices was slightly modified.

* Added new MQTT2Go compatible devices:
	* Shelly 2.5 ([link](https://github.com/mqtt2go/devices/tree/master/Shelly%202.5)) blinds controller,
	* Sonoff B1 ([link](https://github.com/mqtt2go/devices/tree/master/Sonoff%20B1)) smart bulb,
	* Sonoff POW R2 ([link](https://github.com/mqtt2go/devices/tree/master/Sonoff%20POW%20R2)) smart meter,
	* Sonoff S26 ([link](https://github.com/mqtt2go/devices/tree/master/Sonoff%20S26)) smart socket,
	* Sonoff TH16 ([link](https://github.com/mqtt2go/devices/tree/master/Sonoff%20TH16)) multi sensor.

