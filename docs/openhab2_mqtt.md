# OpenHab2 - MQTT binding demo

This demo demonstrates how to use MQTT binding addon in OpenHAB2 installed into Orange Pi zero.
The idea is very basic. A switch is created on default sitemap. When you turn on the switch, it sends "ON" message to Topic `/office/light` via MQTT broker that is installed on Orange Pi. When you turn off the switch, it sends "OFF" message to Topic `/office/light`.


## Require

[Openhab2](http://docs.openhab.org/installation/index.html)

[MQTT broker installed.](https://agrinode.github.io/docs/mqtt_demo/)

### First, install MQTT binding via paperui

Define all the brokers which you want to connect to, in your `services/mqtt.cfg` file.

```
cd /etc/openhab2/services

sudo nano mqtt.cfg

```
The file as follow

```
#
# Define your MQTT broker connections here for use in the MQTT Binding or MQTT
# Persistence bundles. Replace <broker> with an ID you choose.
#

# URL to the MQTT broker, e.g. tcp://localhost:1883 or ssl://localhost:8883
mosquitto.url=tcp://localhost:1883

# Optional. Client id (max 23 chars) to use when connecting to the broker.
# If not provided a default one is generated.
mosquitto.clientId=lion

# Optional. User id to authenticate with the broker.
#<broker>.user=<user>

# Optional. Password to authenticate with the broker.
#<broker>.pwd=<password>

```

### Define demo item in `demo.items`

`Switch mySwitch {mqtt=">[mosquitto:/office/light:command:ON:1],>[mosquitto:/office/light:command:OFF:0]"}`

### Define sitemap

```
sitemap default label="My first sitemap"
{
    Switch item=mySwitch label="Office Light"
    
}

```
![openhab2 mqtt binding](https://2.bp.blogspot.com/-LHjyzaVBaEg/WPzjTu1rN8I/AAAAAAAAJj4/WiHRZgHBnX4325mCCe2aogpydlbHqEVxQCLcB/s400/openhab2%2Bmqtt%2Bbinding.png)

### Show log file
Open terminal, then run:

`tail -f /var/log/openhab2/openhab.log`

<div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/2jcirF04EU0?ecver=2" width="640" height="360" frameborder="0" style="position:absolute;width:100%;height:100%;left:0" allowfullscreen></iframe></div>