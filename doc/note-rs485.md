# RS485 Testing Note

## Device name

The RS485 tranceiver is connected on the third port of USB hub chip.
It appears as `/dev/ttyUSB0` and it is consistant over different boards and OS revisions.

```
                 ┌───────────┐
                 │           │      ┌────────┐  /dev/ttyUSB2  ┌───────┐
                 │      USB0 ├─────►│ CP2102 ├───────────────►│ UHF   │
                 │           │      └────────┘                └───────┘
                 │           │
                 │           │      ┌────────┐  /dev/ttyUSB1  ┌───────┐
┌────────────┐   │      USB1 ├─────►│ CP2102 ├───────────────►│ RS232 │
│            │   │           │      └────────┘                └───────┘
│   Omega2   ├───┤USBHub     │
│            │   │           │      ┌────────┐  /dev/ttyUSB0  ┌───────┐
└────────────┘   │      USB2 ├─────►│ CP2102 ├───────────────►│ RS485 │
                 │           │      └────────┘                └───────┘
                 │           │
                 │           │
                 │      USB3 ├─────┐
                 │           │     │       ┌──────────────────┐
                 └───────────┘     │       │ 4G Modem         │
                                   └───────┤ USB              │
                                           │ via mini-PCIe    │
                                           └──────────────────┘
```

## DE/RE Control

For *Driver Enable* and *Receiver Enable* controlling, **RTS** pin of CP2102 is used.
Note that the **RTS** pin has reversed logic.
Thus, for transmission, set **RTS** value as 0.
For receiving, set **RTS** value as 1.

From `pyserial` version 3.0, `Serial.setRTS` is deprecated.
Instead, it is recommended to use getter and setter for `rts` property.
The following example shows how to transmit data using `pyserial`

```py
import serial
rs485 = serial.Serial('/dev/ttyUSB0', 9600, rtscts=False)  # port is opened here
# default RTS value is 1
# RTS# is LOW
rs485.close()  # close port
rs485.rts = 0  # for transmitting mode
rs485.open()   # reopen the port, now RTS# pin is HIGH.
rs485.write(b'Hello RS485')
```
