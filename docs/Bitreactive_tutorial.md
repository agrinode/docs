# Tutorial: Build a Configurable Eclipse Kura Application via Reactive Blocks

## Summary. 

This tutorial demonstrates how to build a Kura application into a bundle by using Reactive Blocks.

## About Reactive Blocks.

Bitreactive provides the visual development environment that allows java users build their own IoT applications by simply connecting building blocks. Each Reactive Block is designed for a specific purpose such as connecting to Cloud platform (IBM Watson IoT, Xively,...), providing MQTT protocol, LoRaWAN interface,...
Reactive Blocks is a Plugin for Eclipse, which provides a development environment for a developer build a complex application by drag and drop reactive blocks from the Reactive Blocks libraries.
For me, I have a very basic knowledge of Java but I can build a complex java application with Reactive Blocks. Each block has a description in the form of input and output pins and contract. In addition, Reactive Blocks provides the analyze and animation tools which are helpful for testing the applications.

### References:

Block by Block Towards IoT Applications
Integrating Reactive Block with JamaicaVM

## About "AgriNode bundle".

In this post, I share the workflow of "AgriNode" bundle that will be installed into Eclipse Kura based gateway. The gateway has MQTT server which sensor nodes connect to.
The bundle purpose is to send a command for getting data to sensor nodes via MQTT (with a gateway server: 172.16.1.1:1883). After receiving the command from the gateway, sensor nodes take sensors reading then send data back to the gateway. The gateway then forwards data to the cloud via MQTT protocol (with another broker: blocks2.bitreactive.com).

### Requierment.
Eclipse
Install Reactive Blocks for Kura

## Build the Agrinode bundle with Reactive Blocks.

![](https://2.bp.blogspot.com/-iT9vqyTUkS0/WJ6ouBuWIlI/AAAAAAAAIWQ/x_zYRPVGPf4O2vDS137Ogx939NxRC7bkwCLcB/s640/Untitled.png)

* Block 1 - Periodic Toggler: To emit a signal (to publish a command message to sensor nodes) every configurable duration, The duration is configurable through Eclipse Kura configuration.

* Block 2 - MQTT Publish: This block publishes messages on the local MQTT Broker (172.16.1.1:1883). "init" function:

~~~~
public MQTTConfigParam init() {
MQTTConfigParam p = new MQTTConfigParam("172.16.1.1");
return p;
}
~~~~

* Block 3 - MQTT subscriber: This block subscribes to messages from sensor nodes (node01, node02,...) on the local MQTT Broker (172.16.1.1:1883). "init1" function:

~~~~
public MQTTConfigParam init1() {
MQTTConfigParam p1 = new MQTTConfigParam("172.16.1.1");
p1.addSubscribeTopic("node02_data");
p1.addSubscribeTopic("node01_data");
return p1;
}
~~~~

* Block 4 - MQTT Publish: This block publishes messages on the cloud MQTT Broker (blocks2.bitreactive.com). "init2" function:

~~~~
public MQTTConfigParam init2() {
MQTTConfigParam p2 = new MQTTConfigParam("blocks2.bitreactive.com");
return p2;
}
~~~~


## Deploy the bundle

First, build the project for Kura --> quick build to make a *.dp file --> install *.dp file via Kura Web ui.

![](https://2.bp.blogspot.com/-YXpSPIdixco/WJ6spp5r59I/AAAAAAAAIWc/d-24LGz1t2UDD-HYEXk-PPu3Ek2k0UTcACLcB/s640/agrinode_kura.png)
## Test result.
The data is now available at http://agrinode.mybluemix.net/ui/#/0

The bundle works fine but there is an error that needs to be fixed (I am working on it).
The problem is that the bundle will not send any data to the cloud and can not reconnect to the cloud broker if the internet connection lost occurs. The Block 4 will disconnect and not reconnect to the broker if there is any error.  The only way to get the bundle works again is to reboot the gateway.
The picture below figures out this issue:

![](https://2.bp.blogspot.com/-KIRteANHbMw/WJ6vEHf4tmI/AAAAAAAAIWo/ZjkWqiyE1gAaShzpTlChRBDlspn51uv5ACLcB/s640/agrinode_bitreactive_result.png)