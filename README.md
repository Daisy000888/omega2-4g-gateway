# TowerController III: Omega2 4G Gateway

Sponsored by [TowerSoftware](http://www.towersoftwareltd.com/)

<img src="https://github.com/hotteshen/omega2-4g-gateway/blob/develop/doc/image/preview-3d.png?raw=true" style="width: 50%;">

<pre style="line-height: .8em; font-size: 1em;"><code>
┌──────────────────────TowerControllerIII─────────────────────┐
│                                                             │
│   ┌────Hardware───┐  ┌───────OS──────┐  ┌─────WebApp────┐   │
│   │   (Omega2S+)  │  │    (OpenWRT)  │  │    (Python)   │   │
│   │               │  │               │  │               │   │
│   │   4G / UHF    │  │   Drivers     │  │   WebSSH      │   │
│   │   GNSS        │  │   Routers     │  │   Bus Ctrl    │   │
│   │   CAN         │  │               │  │   FW Update   │   │
│   │   RS232       │  │               │  │               │   │
│   │   RS485       │  │               │  │               │   │
│   └───────────────┘  └───────────────┘  └───────────────┘   │
└─────────────────────────────────────────────────────────────┘
</code></pre>

Onion Omega2 4G Gateway with UHF and precise GNSS module, running OpenWRT
* Onion Omega2S+
  - MIPS23kEc procesor@ 580MHz
  - 128MB DDR2 RAM
* Hemisphere Vega 28 Precision GNSS Module
* Satelline M3-TR3 UHF Transceiver Module
* Simcom SIM7600 4G Modem in a Mini-PCIe Form


## Milestones

* [ ] Hardware
  - [x] Omega2
  - [x] USB Debug
  - [x] Ethernet 0
  - [x] Ethernet 1
  - [x] GNSS
  - [ ] UHF modem
  - [x] 4G modem
  - [x] RS232
  - [ ] RS485
  - [x] CAN
  - [x] System power
  - [x] 12V fixed power out
  - [ ] Adjustable power out
* [ ] OS
  - [x] Build environment
  - [ ] Drivers
    - [x] Ethernet 0
    - [x] Ethernet 0 activity LEDs
    - [ ] Ethernet switch
    - [ ] GPIO
    - [x] CP2102 UART driver
    - [x] I2C driver
    - [x] SPI driver
    - [x] MCP2515 CAN driver
  - [ ] Router
    - [ ] Wifi
    - [ ] 4G
    - [ ] UHF
* [ ] WebApp
  - [x] HTML design
  - [x] WebSSH
  - [ ] API server skeleton
  - [ ] CAN/RS232/485
  - [ ] GNSS
