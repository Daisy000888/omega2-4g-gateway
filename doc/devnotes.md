# Onion Omega2 Dev Notes

## OpenWRT upgrade

Download the latest OpenWRT sysupgrade binary and run `sysupgrade` command with it.


## Enabling USB

Install USB kernel modules and utils

```
# opkg update
# opkg install kmod-usb-storage
# opkg install kmod-usb-core
# opkg install kmod-usb-uhci
# opkg install kmod-usb2
* opkg install kmod-usb-serial-cp210x
# opkg install usbutils
```

Now `lsusb` command is working.

```
# lsusb
Bus 001 Device 004: ID 10c4:ea60 Silicon Labs CP2102 USB to UART Bridge Controller
Bus 001 Device 006: ID 10c4:ea60 Silicon Labs CP2102 USB to UART Bridge Controller
Bus 002 Device 001: ID 1d6b:0001 Linux 5.10.146 ohci_hcd Generic Platform OHCI controller
Bus 001 Device 007: ID 2c7c:0125 Android Android
Bus 001 Device 005: ID 10c4:ea60 Silicon Labs CP2102 USB to UART Bridge Controller
Bus 001 Device 002: ID 1a40:0101  USB 2.0 Hub
Bus 001 Device 001: ID 1d6b:0002 Linux 5.10.146 ehci_hcd EHCI Host Controller
```

Listed devices are,
* CP2102 for RS485
* CP2102 for RS232
* CP2102 for UHF modem
* `2c7c:0125` is Quectel EC25


## Enabling UART1 and UART2 to talk to GNSS module

(todo)


## Ethernet Setup

### ETH0

Enabled by default. No need to change.

* `ETH0_LINK` - GPIO42, ACIVE_LOW
* `ETH0_ACT` - GPIO43, ACIVE_LOW

### ETH1

Gateway-ed port for GNSS
(todo)

* `ETH1_LINK` - GPIO39, ACIVE_LOW
* `ETH1_ACT` - GPIO40, ACIVE_LOW

### ETH2

Not used. Disabled by default. No need to change.

### ETH3

Connected to GNSS.
(todo)
