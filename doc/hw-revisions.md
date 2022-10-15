# Hardware Revisions

## TowerController V3

### Tested Working

* [x] Omega2 USB Serial debugging
* [x] Main Ethernet port
* [x] 4G mini-PCIe module
* [x] RS232
* [x] 12V power out
* [ ] UHF module
  - have no UHF module to test
* [ ] GPS module
  - in progress
* [ ] RS485
  - in progress
* [ ] CAN
  - in progress
* [ ] AUX adjustable power out
  - can test after I2C pull up resistor fixed

### Discovered HW Issues

* [ ] 3.3V power supply has ~0.5V ripple noises when the GPS module is connected. Without GPS, it's fine. Need to add power filter for GPS module.
* [ ] I2C bus has missing pull up resistors. Need to add 2.2~3.3k resistors
* [ ] GPS's Ethernet and the secondary Ethernet port has wrong connection to Omega2's GPIO pins. Need to detach Omega2 from GPS Ethernet controlling.
