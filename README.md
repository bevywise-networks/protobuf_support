# Extensions for ProtoBuf support

This file documents the steps to add Python support for MQTT payload of ProtoBuf type in Bevywise IoT Simulator and Bevywise MQTT broker.

## ProtoBuf
_Protocol buffers_ are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data.

### Compile  .proto file
1. Write .proto file according to the requirement (screen.proto is used as an example here).
2. Compile the .proto file using protobuf compiler. 
		$ protoc --python_out=. screen.proto
		The compiler generates a Python file.(screen_pb2.py)
	(Note:) Protobuf compiler can be installed using:
		$ sudo apt install protobuf-compiler

## Bevywise MQTTRoute
Bevywise MQTTRoute is a highly extendable & reliable MQTT Broker fuelling powerful data management to build large scale IoT applications / solutions.
https://www.bevywise.com/mqtt-broker/

### Add ProtoBuf support to MQTTRoute

1. Copy the compiled protoBuf Python class file (eg. screen_pb2.py) to Bevywise/MQTTRoute/extensions/
2.  Copy custom_store.py to Bevywise/MQTTRoute/extensions/
3. In the  the method "handle_Received_Payload()" in Bevywise/MQTTRoute/extensions/custom_store.py, replace the imported ProtoBuf class name with the one used. If required this method can be modified. (Note: If the sample screen_pb2.py is used, modifications to custom_store.py are not required.)
4. Set CUSTOMSTORAGE = ENABLED in Bevywise/MQTTRoute/conf/data_store.conf
5. Google protobuf needs to be installed in MQTTRoute/lib. This can be done by either copying the directory 'google' into MQTTRoute/lib or by installing it via pip into the lib folder ($ pip install protobuf -t MQTTRoute/lib)
6. Run MQTTRoute
	In Linux based machines,
	$ cd Bevywise/MQTTRoute/bin
	$ sh runbroker.sh
	
## Bevywise IoT Simulator
Bevywise IoT Simulator is a simulation tool to simulate tens of thousands of MQTT clients in a single box.
https://www.bevywise.com/iot-simulator/

### Add ProtoBuf support to IoT Simulator

1. Copy the compiled protoBuf class file (eg. screen_pb2.py) to Bevywise/IoTSimulator/lib folder.
2.  Copy default_interceptor.py to Bevywise/IoTSimulator/extensions/
3. In the  the method "on_before_send()" in Bevywise/IoTSimulator/extensions/default_interceptor.py, replace the imported ProtoBuf class file with the one used. If required this method can be modified. (Note: If the sample screen_pb2.py is used, modifications to default_interceptor.py are not required.)
4. Run IoTSimulator 
	In Linux based machines,
	$ cd Bevywise/IoTSimulator/bin
	$ sh runsimulator.sh
5.  In Simulator UI, enable Interceptor and point to interceptor file, by choosing 'Enabled' on Settings(Gear Icon)->Advanced->Python Interceptor. If the default interceptor is not used, then the file path to the new file should be put in 'Python File' text box.
6. In Simulator UI, choose/create a device, and an event that corresponds to the .proto file.
	- Choose a device
	- Click on '+' and choose an event (eg. Instant)
	- Enter a topic name. 
	- Select JSON as Message Type. Enter keys and values corresponding to the .proto file used.
	For the sample screen_pb2.py, paste the following into the message text area
	{"luminance": "100-1000-RANGE", "color_depth": "65M-CONSTANT", "temperature": "1-100-RANGE", "ntsc": "45-60-RANGE", "contrast_ratio": "200:1-CONSTANT", "backlight_longevity": "20000-60000-RANGE", "touch_type": "capacitive-CONSTANT", "viewing_tech": "oled-CONSTANT"}


