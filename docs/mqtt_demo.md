
<a href="https://iot.eclipse.org/open-iot-challenge/images/iot-challenge-3-promo.png" style="clear: left; float: left; margin-bottom: 1em; margin-right: 1em;"><img border="0" height="103" src="https://iot.eclipse.org/open-iot-challenge/images/iot-challenge-3-promo.png" width="200" /></a>
## <a href="http://mosquitto.org/">Mosquitto&nbsp;</a>(MQTT broker) Demo</h2>

MQTT stands for Message Queuing Telemetry Transport which is an ISO standard (ISO/IEC PRF 20922) publish-subscribe-based "lightweight" messaging protocol for use on top of the TCP/IP protocol. It is designed for connections to remote locations where a "small code footprint" is required or the network bandwidth is limited. The publish-subscribe messaging pattern requires a message broker. The broker is responsible for distributing messages to interested clients based on the topic of a message. Andy Stanford-Clark and Arlen Nipper of Cirrus Link Solutions authored the first version of the protocol in 1999 [[1]](https://en.wikipedia.org/wiki/MQTT). 

### In this demo, I work on Mosquitto platform (An Open source MQTT v3.1/v3.1.1 Broker)

The picture shows the basic MQTT protocol. MQTT broker - Mosquitto is installed into a Gateway - [Kura based](http://www.eclipse.org/kura/) (Hardware: Raspberry Pi 2; OS: Raspbian). The clients - Publisher/Subscriber connect to MQTT broker via WiFi which is established by the PC. The Subscriber is an Arduino board with WiFi module that subscribes a Topic (for example "Node01") to receive a message. The Publisher is a smartphone running MQTT client software that will publish a message to a topic (for example "Node01").

![](https://2.bp.blogspot.com/-tA0MEjY4_ec/WF3lFYjoFnI/AAAAAAAAABU/DZtniJ4-VtsdwiDxw98Kf7U2Xm1sC5o6QCLcB/s400/agrinode%2BMQTT%2Bdemo.png)

## Requirements

* Hardwares: Raspberry Pi 2 (WiFi dongle); Arduino with WiFi module (Adafruit Huzzah Esp8266); Smartphone (Android, IOS)

## * Step 1: Setup Kura into Raspberry Pi

The picture shows the Gateway hardware which consists of Raspberry Pi and WiFi dongle.

![](https://files.slack.com/files-pri/T3DCSETTK-F3M8CT37U/img_20170104_224654.jpg?pub_secret=f7bf6651fb)

Follow this link for installing Kura into Raspberry Pi: [http://eclipse.github.io/kura/doc/raspberry-pi-quick-start.html](http://eclipse.github.io/kura/doc/raspberry-pi-quick-start.html)

We setup the Raspbery Pi in the Access Point mode for providing WiFi connection:

![](https://files.slack.com/files-pri/T3DCSETTK-F3MUBSZML/untitled.png?pub_secret=a5706e31ba)


## * Step 2: Install Mosquitto broker into Raspberry Pi

We use Mosquitto platform as a MQTT broker running on Rapsberry Pi. It provides MQTT protocol for our sensor network system. For more information about MQTT protocol; please visit this page: [what is MQTT and how does it work](https://www.ibm.com/developerworks/mydeveloperworks/blogs/aimsupport/entry/what_is_mqtt_and_how_does_it_work_with_websphere_mq?lang=en)

To install Mosquitto, follow these steps:

`wget http://repo.mosquitto.org/debian/mosquitto-repo.gpg.key`

`sudo apt-key add mosquitto-repo.gpg.key`

Then make the repository available to apt:

` cd /etc/apt/sources.list.d/`

Then one of the following, depending on which version of debian you are using:

` sudo wget http://repo.mosquitto.org/debian/mosquitto-wheezy.list
sudo wget http://repo.mosquitto.org/debian/mosquitto-jessie.list`
 

Then update apt information:

` apt-get update`

And discover what mosquitto packages are available:

` apt-cache search mosquitto`

Or just install:

 `apt-get install mosquitto`

For more information, please follow this link: [http://mosquitto.org/2013/01/mosquitto-debian-repository/](http://mosquitto.org/2013/01/mosquitto-debian-repository/)


## * Step 3: Install MQTT demo code for Arduino

The Huzzah esp8266 board will receive the message from the phone in order to control a LED (connected to pin #0). The LED will be turned off if Huzzah receives message "a" and turn on with others messages.

![](https://files.slack.com/files-pri/T3DCSETTK-F3MB79KL4/img_20170104_233613.jpg?pub_secret=a967d9fcc5)


There are many MQTT libraries for Arduino platform. For this demo, I use MQTT library created by Joel Gahwiler [(available in Github)](https://github.com/256dpi/arduino-mqtt)

It is installed into Ardafruit Huzzah Esp8266 board.

In the sketch, we need to define these parameters:

"firstly, we need to connect Arduino Huzzah to the Gateway via WiFi connection which is established by the Gateway"

`ssid = "YOUR_WIFI_NAME"`   //WiFi connection established from the Gateway

`pass = "YOUR_WIFI_PASSWORD"` 

Then replace `"broker.shiftr.io"` by `172.16.1.1` for MQTT broker

Finally, define subscribe topic `client.subscribe("node01");` for receiving message payload from the publisher.

### Full code here:

`#include <ESP8266WiFi.h>`
`#include <MQTTClient.h>`

`const char *ssid = "agrinode";`
`const char *pass = "12345678";`

`WiFiClient net;`
`MQTTClient client;`

`unsigned long lastMillis = 0;`

`void connect(); // <- predefine connect() for setup()`

`void setup() {`
  `Serial.begin(9600);`
  `WiFi.begin(ssid, pass);`
  `client.begin("172.16.1.1", net);`
  `pinMode(0,OUTPUT);`
  `connect();`
`}`

`void connect() {`
 ` Serial.print("checking wifi...");`
` while (WiFi.status() != WL_CONNECTED) {`
 `   Serial.print(".");`
  `  delay(1000);`
 ` }`

 ` Serial.print("\nconnecting...");`
  `while (!client.connect("arduino", "try", "try")) {`
  `  Serial.print(".");`
   ` delay(1000);`
 ` }`

  `Serial.println("\nconnected!");`

 ` client.subscribe("node01");`
  `// client.unsubscribe("/example");`
`}`

`void loop() {`
 ` client.loop();`
  `delay(10); // <- fixes some issues with WiFi stability`

 ` if(!client.connected()) {`
 `   connect();`
 ` }`

 ` // publish a message roughly every second.`
 ` if(millis() - lastMillis > 1000) {`
 `  lastMillis = millis();`
 `   client.publish("/hello", "world");`
  `}`
`}`

`void messageReceived(String topic, String payload, char * bytes, unsigned int length) {`
`  Serial.print("incoming: ");`
`  Serial.print(topic);`
`  Serial.print(" - ");`
 ` Serial.print(payload);`
 ` Serial.println();`
 ` if (payload=="a"){digitalWrite(0,HIGH);}`
  `else digitalWrite(0,LOW);`
`}`

## * Step 4: Install MQTT client software for Android phone

There are many softwares available for MQTT protocol testing. I use [MyMQTT](https://play.google.com/store/apps/details?id=at.tripwire.mqtt.client) for my Android phone.

![](https://lh5.ggpht.com/R7Z4Cr_Mze_EEzR0bZKg40aUqowmynAAcFxEUOnajwvg4Y4005lL1NOrW4UrPAXrFmI=h900-rw)

For "Setting": we need MQTT broker host (it is the Gateway IP: `172.16.1.1:1883`) and "Topic" for publishing message (it is: `Node01`).

For publishing a message: navigate to "Publish" button -> then fill in the Topic (`Node01`) and message.

## * Demo Video


## Refferences

[1] [https://en.wikipedia.org/wiki/MQTT](https://en.wikipedia.org/wiki/MQTT)