[Back](./index.md#add-devices)
# Setup new MQTT2GO Controller
The MQTT2GO Controllers are vital part of any MQTT2GO-enabled installation. They are the entry point for users that want to interact with the whole MQTT2GO ecosystem. This is true for all the MQTT2GO installation no matter if there is or is not the local MQTT2GO broker.

From the perspective of the whole MQTT2GO communication infrastructure, the setup of the Controller brings one new element that have to be utilized. It is the Management Server, which has to be operated by the MQTT2GO adopter and which contains all the neccessary information about the MQTT Brokers (ip addresses, names) and Users. These users have to be created by the adopter authority as it is the only way how to maintain the security.

This section defines how the creation of a new MQTT2GO account and addition of a new MQTT2GO Controtroller is done.

## Creation of new MQTT2GO account
The new MQTT2GO account creation proccess utilizes credentials provided by the MQTT2GO standard adopter entity. They are meant to be provided after the initial client registration proccess to the adopter's system. The creation proccess steps are following:

1. The MQTT2GO App / Web Interface connects to the management server via __HTTPS: /admin_login__ using the provided credetials (Username and Password).
1. The MQTT2GO APp / Web Interface sends a request containing the Username, Password, Phone Number, Home ID, and GW ID to the Management server on the  __HTTPS: /add_user__.
1. Management server forwards this information to the correct MQTT2GO cloud broker, which then creates the user.

<p align="center" >
	<img src="create_account.svg" alt="Proccess of creating new MQTT2GO account">
</p>
<p align="center" >
	<a name="create-acocunt-fig"></a><em><strong>Fig. 1:</strong> Proccess of creating new MQTT2GO account.</em>
</p>

## Creation of new MQTT2GO controller
The MQTT2GO controller creation proccess can be initialized only if at least one MQTT2GO account creation was successfull. If this requirement is satisfied, created user is able to add a new MQTT2GO controller and users to the system. It is also worthwile to be noted, that in theor users can use the same username for multiple logins, but to deal with this problem is not in scope of the MQTT2GO standard. The MQTT2GO creation proccess is again utilizing the Management Server for user authentication. The reason to utilize it together with the Cloud MQTT2GO Broker is mainly the security. The process itself can be described in following steps:
1. The MQTT2GO App tries to connect to __HTTPS: /user_login__ using the user credentials (username and password).
1. If the credentials are valid, the Management Server forwards this request to the __HTTPS: / login_user__ of Cloud MQTT2GO Browser and initializes the login login proccess.
1. The Cloud MQTT2GO Broker then sends activation code via the SMS for client verification.
1. User enters this code into the MQTT2GO App which then sends it into the __HTTPS: /get_credentials__ of the Management Server.
1. The Management server forwards the activation code to the __HTTPS: /get_credentials__ of Cloud MQTT2GO Broker.
1. The Cloud MQTT2GO Broker then sends the certificate to the __HTTPS: /post_credentials__ of the Management Server.
1. The Management server then sends a response with User ID, Broker IP, and Certificate to the __HTTPS: /get_credentials__ from which the MQTT2GO App saves it.
1. The MQTT2GO App connects to the Cloud MQTT2GO Broker using the provided __certificate__,  __user ID__, __login__, __password__, and __broker IP__.
1. The MQTT2GO App subscribes to the __\<user_id\>/topics__ and publishes a __GET_DEVICE_TOPICS__ message.
1. The Cloud MQTT2GO Broker publishes to the __\<user_id\>/topics__ message with all topics the MQTT2GO App has to subscribe to.
1. From now on, the MQTT communication follows the MQTT2GO standard.

<p align="center" >
	<img src="mqtt_controller_login.svg" alt="Proccess of login into MQTT2GO account">
</p>
<p align="center" >
	<a name="add-devices-fig"></a><em><strong>Fig. 2:</strong> Proccess of login into MQTT2GO account (adding new MQTT2GO controller).</em>
</p>

This procedure is presented as the ideal implementation of the controller creation. If the user wants to utilize third party MQTT client, the green part (Authentication) that is exploited for broker IP and certificate obtaining has to implmented or used separately (i.e., using the web browser). 

### User authentication
The user authentication operation inside MQTT2GO controller creation is utilizing the HTTPS API and therefore does not follow the MQTT2GO topic naming convention. The reason for this is to simplify the access proccess of a service, which will be utilized only a several times. 

## HTTPS API message structure
Eventhough the HTTPS API is not adhering to the topic naming convention, it still utilizes the JSON data structure of the messages. This section provides examples of all utilized messages.

### user_login
This message contains information about the user credentials. Its structure is following:
```json
{	
	"username": "username",
	"password": "pwd"
}
```

## Configuration
This section is utilizing the standard MQTT2GO topic naming structure together with standard MQTT2GO messages. They are described in following section.

### MQTT Commands
In this specific implementation, the only MQTT2GO command is only one and its primary goal is to request all device topics to which the controller have to subscribe. This secures the controller to be able to controll all devices, to which the selected user has access to. The command structure is based on the structure from <a href="./mqtt2go-commands#mqtt_commands">MQTT Commands</a>. The numbering in this section is coherent with the numbering in <a href="#add-devices-fig">Fig. 2</a>.

#### Get Device Topics
<p align="justify">
Get device topics command (1) is used to get device topics from the SH-GW. This command has value of <em>GET_DEVICE_TOPICS</em>.
</p>

```json
{
	"type": "command",
	"timestamp": "timestamp_value",
	"command_type": "topics",
	"value": "GET_DEVICE_TOPICS"
}
```

### MQTT Reports
<p align="justify">
The MQTT reports presented here are designed as a “responses” to aforementioned commands. Their structure is also coherent with the general structure from <a href="./mqtt2go-commands#mqtt_reports">MQTT Reports</a> and the numbering is matching the one in <a href="#add-devices-fig">Fig. 2</a>.
</p>

#### Device Topics
<p align="justify">
This report (2) is used to deliver the requested topics, in which the new device is intended to subscribe.
</p>

```json
{
	"type": "report",
	"report_type":"command_response",
	"timestamp": "timestamp_value",
	"report_name": "topics",
	"value": ["topic_1", "topic_2", "topic_3"]
}
```

[Back](./index.md#add-devices)
