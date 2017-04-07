### Orange Pi Zero

Orange Pi Zero is a cheap single-board computer. It can run Android 4.4, Ubuntu, Debian.  It uses the AllWinner H2 SoC, and has 256MB/512MB DDR3 SDRAM(256MB version is Standard version

![agrinode orange pi zero](http://www.orangepi.org/orangepizero/images/orangepizero_info.jpg)

Order here: [http://www.orangepi.org/orangepizero/](http://www.orangepi.org/orangepizero/)

### Install Debian on Orange Pi

Please follow the link: [https://docs.armbian.com/User-Guide_Getting-Started/](https://docs.armbian.com/User-Guide_Getting-Started/)

### Install default java JRE/JDK on Debian (JDK 7)

* Web Browser Plugin

To install the default Web Browser Plugin on your system, run: 

```
apt-get install icedtea-plugin
```

* JRE
To install the default JRE (Java Runtime Environment) on your system, run: 

```
apt-get install default-jre
```

* JDK

To install the default JDK (Java Development Kit) on your system, run: 

```
apt-get install default-jdk
```

Source: [https://wiki.debian.org/Java](https://wiki.debian.org/Java)

### Or you can install Oracle JDK 8

```
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | sudo tee /etc/apt/sources.list.d/webupd8team-java.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | sudo tee -a /etc/apt/sources.list.d/webupd8team-java.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
sudo apt-get update
sudo apt-get install oracle-java8-installer
sudo apt-get install oracle-java8-set-default
```

### Install Eclipse Kura

OrangePi runs Debian 8. It is compatible with [Kura pakage for BeagleBone](http://eclipse.github.io/kura/doc/beaglebone-quick-start.html)

* Install the gdebi command line tool:

```
sudo apt-get update
sudo apt-get install gdebi-core
```

* Download the Kura package:

```
wget http://download.eclipse.org/kura/releases/<version>/kura_<version>_beaglebone_debian_installer.deb
```

Note: replace <version> in the URL above with the version number of the latest release (e.g. 2.1.0).

* Install Kura:

```
sudo gdebi kura_<version>_beaglebone_debian_installer.deb
```

* Reboot the Orange Pi

```
sudo reboot
```

* Kura setups a local web ui that is available using a browser via:

```
http://<device-ip>
```

For more detail, please visit: [http://eclipse.github.io/kura/doc/beaglebone-quick-start.html](http://eclipse.github.io/kura/doc/beaglebone-quick-start.html)

Video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhVsEFJdafo" frameborder="0" allowfullscreen></iframe>