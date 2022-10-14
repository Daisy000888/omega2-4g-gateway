# Onion Omega2 Dev Notes

## OpenWRT upgrade

Download the latest OpenWRT sysupgrade binary and run `sysupgrade` command with it.


## Enabling USB

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


## RS232 Loopback Testing

(todo)


## Enabling UART1 and UART2 to talk to GNSS module

(todo)


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
			gpios = <&gpio0 42 GPIO_ACTIVE_LOW>;
			default-state = "off";
		};
		eth0_led1: data {
			label = "omega2p:orange:data";
			gpios = <&gpio0 43 GPIO_ACTIVE_LOW>;
			default-state = "off";
		};
	};
```

### ETH1 & ETH3

These are not physical PHYs, but just connected to the normal GPIO pins

* `ETH1_LINK` - GPIO39, ACIVE_LOW
* `ETH1_ACT` - GPIO40, ACIVE_LOW

(todo)


### Python Setup

```
# opkg update
# opkg install python3 python3-pip
```


### Running Python Web Console

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
