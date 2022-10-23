# Hardware Revisions

## TowerController v3.0a1

### To Test / Tested

(Checked means Tested)

* [x] Omega2 USB Serial debugging
* [x] Main Ethernet port
* [x] 4G mini-PCIe module
* [x] RS232
* [x] 12V power out
* [x] CAN
* [x] Ethernet switch
* [x] GPS module
* [x] RS485
* [ ] UHF module
  - have no UHF module to test
* [ ] AUX adjustable power out
  - can test after I2C pull up resistor fixed

### Discovered HW Issues

(Checked means updated in the next HW revision - v3.0a2)

* [x] [FIX] 3.3V power supply has ~0.5V ripple noises when the GPS module is connected. Without GPS, it's fine. Need to add power filter for GPS module.
* [x] [FIX] I2C bus is missing pull up resistors. Need to add 2.2~3.3k resistors
* [x] [FIX] Ethernet port indicating LEDs pin mapping
* [x] make PCIe connections to mini-PCIe connector for future use

## TowerController v3.0a2

* [ ] 3.3V power supply quality
* [ ] AUX power supply controlled via I2C
* [ ] Ethernet pot Indicaters synchronized with switch status
* [ ] PCIe connections
