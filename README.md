# nrf52_ebms

This is code for the NRF52832 and the NRF52840 for communicating between the EBMS and EBMS Tool (linux and mobile) over BLE. After uploading the firmware, the NRF can be connected to the EBMS using the RX and TX pins chosen in main.c, and the BLE scanner in EBMS Tool should be able to find it and connect. Note that the UART port on the EBMS must be enabled with a baud rate of 115200 for this to work. 

The code can be build with the NRF52 SDK by changing the path in Makefile. There is a variable in the makefile called IS_52832. If it is set to 1, the code is built for the NRF52832, otherwise it is built for the NRF52840. The difference between the two is which files are included from the NRF SDK and whether USB-CDC support is used in main.c. For now USB-CDC is only useful for printing debug information, but later it might be possible to connect EBMS Tool over it.

This is how to connect an STLINK V2 to the NRF52 for uploading the code:

| NRF52         | STLINK V2     |
| ------------- |---------------|
| GND           | GND           |
| VDD           | 3.3V          |
| SDO           | SWDIO         |
| SCL           | SWCLK         |

The first time the code is uploaded mass_erase must be run, and the softdevice must be uploaded. This can be done with the following make rules:

```bash
make mass_erase
```

```bash
make upload_sd
```

after that and after future changes only the code itself needs to be uploaded:

```bash
make upload
```

After the code is uploaded, the NRF52 can be connected to the EBMS in the following way:

| NRF52         | EBMS          |
| ------------- |---------------|
| GND           | GND           |
| VDD           | VCC (3.3V)    |
| Px.y (TX)     | RX            |
| Px2.y2 (RX)   | TX            |

Note that a 10 uF ceremaic capacitor between VCC and GND close to the NRF52 module might be needed if the cables are long. Otherwise the connection can become slow and unstable.

