# Network Config on Omega2

## Emergency Boot

```
sudo ifconfig enp6s0 192.168.8.99
sudo route add default gw 192.168.8.99
```

## Done by Device Tree and Kernel modules

* Ethernet Switch is enabled
* Wi-Fi device is enabled
* WWAN (LTE modem) is enabled


## Ethernet

After fresh new build and first boot, the Ethernet is given a fixed address `192.168.1.1`.
Setting it as DHCP client mode.

```sh
uci set network.lan.proto="dhcp"
uci commit network
/etc/init.d/network restart
```

Ref: [OpenWrt as client device](https://openwrt.org/docs/guide-user/network/openwrt_as_clientdevice)


## Wi-Fi Network Bridge
