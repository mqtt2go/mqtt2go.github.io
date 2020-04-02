[Back](./index.md)

# Changelog

* General bugfixes in device adding procedure:
	* device type is now part of the first message,
	* actvation topic now contains mqtt login, password, and CA certificate hash,
	* updated device topic convention _topics -> topic_,
	* Wi-Fi credentials example has been updated to provide more insights into the process of adding a new MQTT2GO device via guest WiFi, see [Setup via Guest WiFi](./add-wifi.md).

* Updated naming conventions:
	* all key words are now in snake_case format.

* Updated MQTT2GO objects
	* Added subpage with detailed functionality description of _Blinds and Sunscreens_ ([link](./examples/blinds.md)) object examples.
	* Added subpage with detailed functionality description of _Smart Sockets_ ([link](./examples/sockets.md)) object examples. 
	* Added subpage with detailed functionality description of _Multi Sensors_ ([link](./examples/multi_sensors.md)) object examples. 


* Added MQTT2GO Compatible Devices section:
	* Added Shelly Plug S device with link to the FW repository, see [Link](https://github.com/mqtt2go/devices). Additional libraries are needed, see the "includes" in main.cpp.
