## Running Hello_world application ( based on Zephyr RTOS ) on Arduino Due

Zephyr Project goal is to build a best-in-breed small, scalable, real-time operation system (RTOS) for variety of IoT devices, such as X86 boards (Arduino 101, Galileo, Intel Quark D2000 development board,..), ARM boards (96Boards, Arduino Due, CC3200 LauchXL, Curie, NXP FRDM, ST Nucleo,...) ARC Boards, NIOS II Boards, XTENSA Boards.

In this post, we test a Hello_world application ( based on Zephyr RTOS ) on Arduino Due

### Requirement

- Host Computer: Ubuntu 16.04 LTS 64-bit
- Device: Arduino Due

### 1. Setup development environment

- Update your OS

```
$ sudo apt-get update
$ sudo apt-get upgrade
``` 
 
- Install requirements

```
$ sudo apt-get install git make gcc g++ python3-ply ncurses-dev \
      python3-yaml python2.7 dfu-util 
```

- Install the Zephyr SDK

  * Download the latest SDK self-extractable binary.

`$ wget https://github.com/zephyrproject-rtos/meta-zephyr-sdk/releases/download/0.9.1/zephyr-sdk-0.9.1-setup.run`

  * Run the installation binary
 

```
$ chmod +x zephyr-sdk-0.9.1-setup.run
$ ./zephyr-sdk-0.9.1-setup.run
```

* To use the Zephyr SDK, export the following environment variables and use the target location where SDK was installed, type:

```
$ export ZEPHYR_GCC_VARIANT=zephyr
$ export ZEPHYR_SDK_INSTALL_DIR=/opt/zephyr-sdk
```

### 2. Download Zephyr project

`$ git clone https://github.com/zephyrproject-rtos/zephyr.git`

### 3. Building a Hello_World Application

```
$ export ZEPHYR_GCC_VARIANT=zephyr
$ export ZEPHYR_SDK_INSTALL_DIR=/opt/zephyr-sdk
$ cd zephyr 
$ source zephyr-env.sh
$ cd samples/hello_world
$ make BOARD=arduino_due
```

After building an application successfully, the results can be found in the `samples/hello_world/outdir/arduino_due` directory

### 4. Flashing the Zephyr kernel onto Arduino Due

Flashing the Zephyr kernel onto Arduino Due requires the bossa tool.
Install BOSSA tool

`$ sudo apt install bossa-cli`
 
Make sure that you are in the directory `/samples/hello_world`

Connect the Arduino Due to your host computer using the programming port.
Press the Erase button for more than 220 ms.
Press the Reset button so the board will boot into the SAM-BA bootloader.

`$ bossac -p ttyACM0 -e -w -v -b outdir/arduino_due/zephyr.bin`

* If you are unable to upload to Arduino board, please change permission on serial port. Type the following command: 

`chmod a+rw /ttyACM0`

### 5. Check for the output
 
`$ minicom -D /dev/ttyACM0`

Press the Reset button and you should see `“Hello World!”` in your terminal.

![](https://3.bp.blogspot.com/-sdfpFOGi7tU/WS5cdeDYJWI/AAAAAAAAJvs/e_TMiaIYnY0yluF5D6J5oecopdAURbTngCLcB/s400/Screenshot%2Bfrom%2B2017-05-31%2B13-01-38.png)