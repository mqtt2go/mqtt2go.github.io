# MQTT2GO - Project Description

The main goal of the MQTT2GO project is to investigate one of the most emerging Internet of Things (IoT) protocols i.e., Message Queuing Telemetry Transport (MQTT) for Smart Home scenarios in which all the communication is done using the protocol in question. Based on the key findings gained during the thorough market overview, the introduced MQTT-enabled proof-of-concept demonstrator enables to handle both the MQTT devices and MQTT controllers, see the figure "Architecture of MQTT2GO System." As the MQTT protocol provides a lightweight method of carrying out messaging using a publish / subscribe model it is well suited for IoT messaging such as with low power sensors or mobile devices such as smartphones, embedded devices, etc.

<p align="center" >
	<img src="mqtt_architecture.svg" alt="Architecture of MQTT2GO System." width="600"/>
</p>
<p align="center" >
	<em>Architecture of MQTT2GO System.</em>
</p>

What is even more, the introduced MQTT2GO standard manages to handle also the Non-MQTT devices which can be also part of the Smart Home scenario. Relying on the current ICT trends, the MQTT2GO standard introduces two MQTT brokers which are located in the local network (local MQTT broker) and in the remote infrastructure of the service provider (cloud MQTT broker). This innovative concept opens the door to a wide variety of communication scenarios e.g., the configuration of the new smart home sensor can be done via the smartphone which is even not present in the local WLAN. Also, the MQTT2GO brings on the light the security aspects of the wired/wireless communications and introduces innovative principles that are technology-driven but at the same time easy to use for the customers.

The lessons learned gathered during the project development are further revealed in the form of the MQTT2GO requirements. The provided information list all the parties within the Smart Home scenario i.e., MQTT brokers, MQTT controllers (connected applications), and MQTT objects (Smart Home devices). Therefore, the provided information should guide the potential 3rd party developers / customers to easily reach the desired implementation level to work with the introduced MQTT2GO standard.
