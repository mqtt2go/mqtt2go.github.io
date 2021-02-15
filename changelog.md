[Back](./index.md)

# Changelog

* Added new type of virtual MQTT2GO devices
	* Intended for third party devices with MQTT2GO compatibility ensured by translating API.
	* Devices can be purely software.
	* The adopters have to implement a part of the procedure on their side.
* Added new option of MQTT2GO controllers login
	* Devices are automatically logged in by simply scanning the QR code from the TV screen.
	* Requires additional development of MQTT2GO mobile app, which communicates with *Management Server*.
* The examples of MQTT2GO messages were extended
	* Each example contain the message topic and the message body for both commands and reports.
* Added links to the other repositories
	* Smart Home TV Dashboar ([link](https://github.com/mqtt2go/tv-dashboard))
	* Anomaly Reporting App ([link](https://github.com/mqtt2go/mqtt-data-mining-demo))
	* Time Series Builder App ([link](https://github.com/mqtt2go/mqtt-data-mining-build-model-demo))

