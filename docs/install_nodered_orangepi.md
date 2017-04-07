# Install Node-red on Orange Pi

## Step 1: Install NodeJs v7.x

```
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs

```

## Step 2: Install Node-RED

`sudo npm install -g --unsafe-perm node-red`

## Step 3: Start Node-RED

```
root@orangepizero:~# node-red
22 Mar 17:51:35 - [info]

Welcome to Node-RED
===================

22 Mar 17:51:35 - [info] Node-RED version: v0.16.2
22 Mar 17:51:35 - [info] Node.js  version: v7.7.3
22 Mar 17:51:35 - [info] Linux 3.4.113-sun8i arm LE
22 Mar 17:51:37 - [info] Loading palette nodes
22 Mar 17:51:43 - [warn] ------------------------------------------------------
22 Mar 17:51:43 - [warn] [rpi-gpio] Info : Ignoring Raspberry Pi specific node
22 Mar 17:51:43 - [warn] ------------------------------------------------------
22 Mar 17:51:43 - [info] Settings file  : /root/.node-red/settings.js
22 Mar 17:51:43 - [info] User directory : /root/.node-red
22 Mar 17:51:43 - [info] Flows file     : /root/.node-red/flows_orangepizero.json
22 Mar 17:51:43 - [info] Creating new flow file
22 Mar 17:51:43 - [info] Starting flows
22 Mar 17:51:43 - [info] Started flows
22 Mar 17:51:43 - [info] Server now running at http://127.0.0.1:1880/

```

## Step 4: Adding nodes
```
cd $HOME/.node-red
npm install <npm-package-name>

```

replace <npm-package-name> with specific nodes package name that you can find in [nodes library](https://flows.nodered.org/). For example, the folowing commands will install package:  Orange Pi GPIO

`sudo npm install node-red-contrib-opi-gpio`

Restart Node-RED. Then open web-brownser. Navigate to http://orange_pi_ip:1880/

You can see the result as the picture below

![install node-red on orange pi](https://3.bp.blogspot.com/-df-mvOXgI_w/WNLA5BniZXI/AAAAAAAAI5s/ZOvyW16PeVsadO7xlav6twQ7mbohqaY1wCLcB/s1600/Untitled.png)

## Step 5 (optional): Start Node-RED on boot

Install nodered.service file and start and stop scripts.

```
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/nodered.service -O /lib/systemd/system/nodered.service
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/node-red-start -O /usr/bin/node-red-start
sudo wget https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/node-red-stop -O /usr/bin/node-red-stop
sudo chmod +x /usr/bin/node-red-st*
sudo systemctl daemon-reload

```

`sudo nano /lib/systemd/system/nodered.service`

Edit the `nodered.service` file as follow:

```
# systemd service file to start Node-RED
[Unit]
Description=Node-RED graphical event wiring tool.
Wants=network.target
Documentation=http://nodered.org/docs/hardware/raspberrypi.html
[Service]
Type=simple
# Run as root user in order to have access to gpio pins
User=root
Group=root
Nice=5
Environment="NODE_OPTIONS=--max-old-space-size=128"
#Environment="NODE_RED_OPTIONS=-v"
ExecStart=/usr/bin/env node-red-pi $NODE_OPTIONS $NODE_RED_OPTIONS
KillSignal=SIGINT
Restart=on-failure
SyslogIdentifier=Node-RED
[Install]
WantedBy=multi-user.target

```
To then enable Node-RED to run automatically at every boot

```
sudo systemctl daemon-reload
sudo systemctl enable nodered.service

```

It can be disabled by

`sudo systemctl disable nodered.service`