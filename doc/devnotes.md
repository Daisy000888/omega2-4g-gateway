# Onion Omega2 Dev Notes


## OpenWRT upgrade

Download the latest OpenWRT sysupgrade binary and run `sysupgrade` command with it.

### Python Setup

```
# opkg update
# opkg install python3 python3-pip
```

## Hardware Drivers

### Enabling USB and Installing USB-Serial Drivers

On the fresh new OpenWRT installation, the serial ports are not present as `/dev/ttyUSBx`.
`lsusb` command is also not working.

To solve this issue, install USB kernel modules and USB utils

```
# opkg update
* opkg install kmod-usb-serial-cp210x usbutils
```

Now `lsusb` command is working, and serial ports are present under `/dev` folder.

```
# lsusb
Bus 001 Device 004: ID 10c4:ea60 Silicon Labs CP2102 USB to UART Bridge Controller
Bus 001 Device 006: ID 10c4:ea60 Silicon Labs CP2102 USB to UART Bridge Controller
Bus 002 Device 001: ID 1d6b:0001 Linux 5.10.146 ohci_hcd Generic Platform OHCI controller
Bus 001 Device 005: ID 10c4:ea60 Silicon Labs CP2102 USB to UART Bridge Controller
Bus 001 Device 002: ID 1a40:0101  USB 2.0 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux 5.10.146 ehci_hcd EHCI Host Controller
# ls /dev/ttyUSB*
/dev/ttyUSB0  /dev/ttyUSB1  /dev/ttyUSB2
```

Listed devices are,
* `ttyUSB0` : CP2102 for RS485
* `ttyUSB1` : CP2102 for RS232
* `ttyUSB2` : CP2102 for UHF modem

### SPI, I2C

On the fresh new OpenWRT installation, the SPI devices are not present as `/dev/spidev*`.
The SPI device kernel driver should be installed manually.

```
# opkg install kmod-spi-dev kmod-regmap-spi
# insmod spidev
# ls /dev/spi*
/dev/spidev0.1
```

Install the drivers for I2C similar to SPI.

```
# opkg install kmod-i2c-mt7628 kmod-i2c-core kmod-regmap-i2c ikmod-can-mcp251x
# i2cdetect -y 0
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- -- 
...
```

(todo - modifying device tree and installing MCP2515 driver)


## Ethernet Setup

### ETH0

Enabled by default. No need to change.

* `ETH0_LINK` - GPIO42, ACIVE_LOW
* `ETH0_ACT` - GPIO43, ACIVE_LOW

Clone [https://github.com/openwrt/openwrt] and edit `target/linux/ramips/dts/mt7628an_onion_omega2.dtsi` file.

Find `leds` section like the following:

```
	leds {
		compatible = "gpio-leds";

		led_system: system {
			label = "amber:system";
			gpios = <&gpio 44 GPIO_ACTIVE_LOW>;
		};
	};
```

Modify it like this:

```
	leds {
		compatible = "gpio-leds";

		system_led: system {
			gpios = <&gpio1 0 GPIO_ACTIVE_LOW>;
		};
		eth0_led0: link {
			label = "omega2p:green:link";
			gpios = <&gpio 42 GPIO_ACTIVE_LOW>;
			default-state = "off";
		};
		eth0_led1: data {
			label = "omega2p:orange:data";
			gpios = <&gpio 43 GPIO_ACTIVE_LOW>;
			default-state = "off";
		};
	};
```

### ETH1 & ETH3

MT7688 has an Ethernet switch feature supporting up to 5 ports.
Need to disable Omega2's GPIOs and enable Ethernet switch feature.

* `ETH1_LINK` - GPIO39, ACIVE_LOW
* `ETH1_ACT` - GPIO40, ACIVE_LOW

(todo)


## Running Python Web Console

OpenWRT build for Omega2 has its own web console written in Lua.
Here, we're going to install and use another web console written in Python.

Installing [WebSSH](https://github.com/huashengdun/webssh).

```
# opkg update
# opkg install python3-tornado python3-paramiko python3-cryptography
# pip install webssh
```

Test run WebSSH.

```
# wssh --address='0.0.0.0' --port=8000
```


## UART Communication using Python

```
# opkg update
# opkg install python3-pyserial
```

(todo)


## OmegaExpansion

OpenWRT 22.03 doesn't include Onion package repository links in the OPKG config.
Add the following lines to `/etc/opkg/distfeeds.conf` using `vi` text editor.
`nano` or `vim` is not installed on OpenWRT by default. To use one of them, need to install it before using.

```
src/gz omega2_core http://repo.onioniot.com/omega2/packages/core
src/gz omega2_base http://repo.onioniot.com/omega2/packages/base
src/gz omega2_packages http://repo.onioniot.com/omega2/packages/packages
src/gz omega2_routing http://repo.onioniot.com/omega2/packages/routing
src/gz omega2_onion http://repo.onioniot.com/omega2/packages/onion
```

Now update OPKG and we can see Omega2 related packages in the package list.

```
# opkg update
# opkg list | grep OmegaExpansion
pyOmegaExpansion - 0.9-1 - Setup for OmegaExpansion Python Package
python3-omega-expansion - 0.9-1 - Setup for OmegaExpansion Python3 Package
```

There's one issue.
OpenWRT 22.03 has Python version 3.10 and the future updates may have higher Python versions.
At the time of this writing, the Onion package repository is based on OpenWRT 18.06, which has Python 3.6.
Thus, Python packages will be organized like this.
* Python libraries installed via `pip3 install *` will go into `/usr/lib/python3.10/`.
* Python libraries installed via `opkg install python3-*` will go into `/user/lib/python3.10/`, if it's from OpenWRT 22.04 package repository.
* Python libraries installed via `opkg install python3-*` will go into `/user/lib/python3.6/`, if it's from Onion package repository.

In our case, the `OmegaExpansion` is installed in `/user/lib/python3.6` and thus, it cannot be imported from Python3, as it is version 3.10.
For now, we will use Python 2.7 for interacting with Omega2 hardwares on OpenWRT 22.03.
We have Python 3.10 already installed, which made `/usr/bin/python` linked to Python 3.10.
Thus, we need to remove this file before installing Python 2.7 from Onion package repository.
Python 3.10 can still be run by `python3` command after deletion.

```
# python --version
Python 3.10.7
# rm /usr/bin/python
# opkg install python-light pyOmegaExpansion pyOnionI2C
...
# python --version
Python 2.7.18
# python3 --version
Python 3.10.7
```

## CAN Bus testing

```
# opkg install kmod-can-mcp251x
# insmod mcp251x
# opkg install kmod
# depmod -a
# opkg install canutils canutils-cansend canutils-candump ip
# ip link set can0 up type can bitrate 250000
```
