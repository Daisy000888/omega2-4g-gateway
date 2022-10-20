# Hardware Revisions

## TowerController V3

### Tested

* [x] Omega2 USB Serial debugging
* [x] Main Ethernet port
* [x] 4G mini-PCIe module
* [x] RS232
* [x] 12V power out
* [x] CAN
* [x] Ethernet switch
* [ ] UHF module
  - have no UHF module to test
* [ ] GPS module
  - in progress
* [ ] RS485
  - in progress
* [ ] AUX adjustable power out
  - can test after I2C pull up resistor fixed

### Discovered HW issues / better things to have

* [ ] [FIX] 3.3V power supply has ~0.5V ripple noises when the GPS module is connected. Without GPS, it's fine. Need to add power filter for GPS module.
* [ ] [FIX] I2C bus is missing pull up resistors. Need to add 2.2~3.3k resistors
* [ ] [FIX] Ethernet port indicating LEDs pin mapping
* [ ] make PCIe connections to mini-PCIe connector for future use
