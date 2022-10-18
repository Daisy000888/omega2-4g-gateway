# Compiling OpenWRT 22.03

## Packages to include

* `kmod`
* `kmod-can-mcp251x`
* `kmod-can-raw`
* `kmod-i2c-mt7628`
* `kmod-ledtrig-netdev`
* `kmod-spi-dev`
* `kmod-usb-serial-cp210x`
* `canutils`
* `canutils-cansend`
* `canutils-candump`
* `ip`
* `usbutils`
* `i2c-tools`
* `python3`
* `python3-pip`
* `python3-bottle`
* `python3-cryptography`
* `python3-paramiko`
* `python3-tornado`
* `python3-pyserial`


## Notes

```
# opkg install kmod-can-mcp251x
# insmod mcp251x
# opkg install kmod
# depmod -a
# opkg install canutils canutils-cansend canutils-candump ip
# ip link set can0 type can bitrate 250000
```
