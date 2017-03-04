## Node-red tutorial
We built a Node-red based application in IBM Bluemix platform for data stored and visualized. Environmental information can be monitored everywhere if there is the internet available by surfing to the specific web address [http://agrinode.mybluemix.net/ui](http://agrinode.mybluemix.net/ui) to monitor the data. 

### Deploy a Node-red application on IBM Blumix
Please follow this link: [https://nodered.org/docs/platforms/bluemix](https://nodered.org/docs/platforms/bluemix)

### Deploy the node-red flow app for receiving data from the gateway

The figure demonstrates how to receive and process data from each sensor node.

 ![nodered flow](https://3.bp.blogspot.com/-Tz00FfFAsgo/WLVCrHU3JPI/AAAAAAAAIjg/3PfjQhpt-EIQb2-EzaHdETTghhvMUNF8ACLcB/s1600/nodered.png)
 
Node  1 is the MQTT node which subscribes the gateway topic to receive the gateway data.
Node 2 is a mongodb node which is responsible for storing sensor data in mongodb (mLab).
Node 3 is a dashboard node which is used to visualize sensor data as the graphs
Moreover, we built the warning system to warn user if the temperature goes high for example.

