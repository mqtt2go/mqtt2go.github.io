# MQTT2GO - Project Description

The main goal of the MQTT2GO project is to investigate one of the most emerging Internet of Things (IoT) protocols i.e., Message Queuing Telemetry Transport (MQTT) for Smart Home scenarios in which all the communication is done using the protocol in question. Based on the key findings gained during the thorough market overview, the introduced MQTT-enabled proof-of-concept demonstrator enables to handle both the MQTT devices (i.e., sensors) and MQTT controllers, see the <a href="#fig1">Fig. 1</a>. As the MQTT protocol provides a lightweight method of carrying out messaging using a publish / subscribe model, it is well suited for IoT messaging such as with low power sensors or mobile devices such as smartphones, embedded devices, home dashboards, etc.

<p align="center" >
	<img src="mqtt_architecture.svg" alt="Architecture of MQTT2GO System." width="600"/>
</p>
<p align="center" >
	<a name="fig1"></a><em><strong>Fig. 1:</strong> Architecture of MQTT2GO System.</em>
</p>

The introduced MQTT2GO standard, see the <a href="#fig1">Fig. 1</a>, manages to handle also the non-MQTT devices, which can be also part of the Smart Home scenario. Relying on the current ICT trends, the MQTT2GO standard introduces two MQTT brokers, which are located in the local network (local MQTT broker) and in the remote infrastructure of the service provider (cloud MQTT broker) respectively. This innovative concept opens the door to a wide variety of communication scenarios e.g., the configuration of the new smart home sensor can be done via the smartphone, which is even not present in the local WLAN. Also, the MQTT2GO brings to the light the security aspects of the wired / wireless communications and introduces innovative principles that are technology-driven but at the same time easy to use for the customers.

The lessons learned gathered during the project development are further revealed in the form of the MQTT2GO requirements for vendors and/or providers of all the components within the Smart Home scenario i.e., MQTT brokers, MQTT controllers (connected applications), and MQTT objects (Smart Home devices). Therefore, the provided information should guide the potential 3rd party developers / customers to easily reach the desired implementation level to get their products compatible with the introduced MQTT2GO standard.
