---
layout: page
title: Planning
permalink: /hardware/planning/
---

If you are used to program usual computer or smartphone apps, web applications or comparable software, you are probably used to try things out until they start to work.
When dealing with hardware dependent programming, this might become already a bit risky, because you could make an irrevocable error that prevents you from undoing that failure again, although the hardware is possibly still intact.
Then, you are forced to switch to a lower level to remedy the defect for example.
But if you are planning and assembling some hardware, you must not make any mistakes or you might destroy your new IC's or get a set of useless PCB's.
So take your time.

At the same time, this does not mean that you are not allowed to build prototypes on a matrix board or a breadboard.
But if you do that because things are not clear, a preferable way might be to find existing and working similar solutions in the internet.


Technical Specification of the Hardware
---------------------------------------

A good start is to have a clear overview over the capabilities of the hardware that should be used.
In general one needs to know what peripherals should be used to find a microprocessor that meets the requirements.
In this case, the microprocessor ESP8266 is chosen mainly due to its unbeatable price and its Wi-Fi support.
The fact that it is capable to meet our requirements is anticipated, you will get an idea to find a processor for your requirements anyway.
You can find more details for the reasons choosing this device in my related master's thesis.

The ESP8266 is a bit complicated in gathering all the needed data because Espressif Systems, the manufactor of the chip, keeps itself covered or omits a lot of documentation.
This is bothersome on the one hand, but helps to reflect on the important things.

You will find the information that the chip "integrates an enhanced version of Tensilica’s L106 Diamond series 32 bit processor" that is running normally at a speed of 80 MHz.
Cadence, that is the new name of the Tensilica processors manufacturer, declares them as "based on an industry-standard architecture" and "smaller than the ARM7 or Cortex-M3 cores" based on the chosen manufacturing process.
Absolute values for the provided memory are hard to find.
The chip possibly includes 64 KB of instruction RAM, 96 KB of data RAM and 64 KB of ROM.
Aroung 50 KB of these are accessible in heap and data section when ESP8266 is working under the station mode and is connected to the router.
The ROM is not programmable, therefore, user programs must be stored in an external SPI flash that can have a size up to 16 MB (128 Mbit).

Furthermore the chip has 16 GPIO's that are multiplexed with other functions like PWM, SDIO, SPI, UART or I2S.

For a complete list, you can see the table below, that is mainly taken from the ESP8266EX datasheet.
The pin description is fitted a bit to the applications that are really used.

| Pin | Name        | Type | Description / Main Application                     | GPIO   | PWM  | SDIO        | SPI   | HSPI  | UART      | I2S      | Misc       |
| ---:| ----------- | ---- | -------------------------------------------------- | ------ | ---- | ----------- | ----- | ----- | --------- | -------- | ---------- |
|   1 | VDDA        | P    | Analog Power 3.0V ~ 3.6V                           |        |      |             |       |       |           |          |            |
|   2 | LNA         | I/O  | RF Antenna Interface, 50Ω Impedance                |        |      |             |       |       |           |          |            |
|   3 | VDD3P3      | P    | Amplifier Power 3.0V ~ 3.6V                        |        |      |             |       |       |           |          |            |
|   4 | VDD3P3      | P    | Amplifier Power 3.0V ~ 3.6V                        |        |      |             |       |       |           |          |            |
|   5 | VDD_RTC     | P    | NC (1.1V)                                          |        |      |             |       |       |           |          |            |
|   6 | TOUT        | I    | ADC Pin (internal)                                 |        |      |             |       |       |           |          |            |
|   7 | CHIP_PU     | I    | Chip Enable (High: On, Low: Off w/ small current)  |        |      |             |       |       |           |          |            |
|   8 | XPD_DCDC    | I/O  | Deep-Sleep Wakeup                                  | GPIO16 |      |             |       |       |           |          |            |
|   9 | MTMS        | I/O  | HSPI Clock                                         | GPIO14 | PWM2 |             |       | CLK   | UART0_DSR | I2SI_WS  |            |
|  10 | MTDI        | I/O  | HSPI Master In Slave Out                           | GPIO12 | PWM0 |             |       | MISO  | UART0_DTR | I2SI_SD  |            |
|  11 | VDDPST      | P    | Digital/IO Power Supply (1.8V ~ 3.3V)              |        |      |             |       |       |           |          |            |
|  12 | MTCK        | I/O  | HSPI Master Out Slave In                           | GPIO13 |      |             |       | MOSI  | UART0_CTS | I2SI_SCK |            |
|  13 | MTDO        | I/O  | HSPI Chip Select,                                  | GPIO15 | PWM1 |             |       | /CS   | UART0_RTS | I2SO_SCK |            |
|  14 | GPIO2       | I/O  | General Purpose Input / Output                     | GPIO2  |      |             |       |       | *         | I2SO_WS  |            |
|  15 | GPIO0       | I/O  | General Purpose Input / Output                     | GPIO0  |      |             | /CS2  |       |           |          |            |
|  16 | GPIO4       | I/O  | General Purpose Input / Output                     | GPIO4  | PWM3 |             |       |       |           |          | CLK_XTAL   |
|  17 | VDDPST      | P    | Digital/IO Power Supply (1.8V ~ 3.3V)              |        |      |             |       |       |           |          |            |
|  18 | SDIO_DATA_2 | I/O  | SPI / HSPI Hold                                    | GPIO9  |      | SDIO_DATA_2 | /HOLD | /HOLD |           |          |            |
|  19 | SDIO_DATA_3 | I/O  | SPI / HSPI Write Protect                           | GPIO10 |      | SDIO_DATA_3 | /WP   | /WP   |           |          |            |
|  20 | SDIO_CMD    | I/O  | SPI Chip Select 0                                  | GPIO11 |      | SDIO_CMD    | /CS0  |       | UART1_RTS |          |            |
|  21 | SDIO_CLK    | I/O  | SPI Clock                                          | GPIO6  |      | SDIO_CLK    | CLK   |       | UART1_CTS |          |            |
|  22 | SDIO_DATA_0 | I/O  | SPI Master In Slave Out, UART1 Tx                  | GPIO7  |      | SDIO_DATA_0 | MISO  |       | UART1_TXD |          |            |
|  23 | SDIO_DATA_1 | I/O  | SPI Master Out Slave In, UART1 Rx                  | GPIO8  |      | SDIO_DATA_1 | MOSI  |       | UART1_RXD |          |            |
|  24 | GPIO5       | I/O  | General Purpose Input / Output                     | GPIO5  |      |             |       |       |           |          | CLK_RTC    |
|  25 | U0RXD       | I/O  | UART0 Rx                                           | GPIO3  |      |             |       |       | UART0_RXD | I2SO_SD  | CLK_XTAL   |
|  26 | U0TXD       | I/O  | UART0 Tx                                           | GPIO1  |      |             | /CS1  |       | UART0_TXD |          | CLK_RTC    |
|  27 | XTAL_OUT    | I/O  | Crystal oscillator output                          |        |      |             |       |       |           |          |            |
|  28 | XTAL_IN     | I/O  | Crystal oscillator input                           |        |      |             |       |       |           |          |            |
|  29 | VDDD        | P    | Analog Power 3.0V ~ 3.6V                           |        |      |             |       |       |           |          |            |
|  30 | VDDA        | P    | Analog Power 3.0V ~ 3.6V                           |        |      |             |       |       |           |          |            |
|  31 | RES12K      | I    | Series with 12 kΩ resistor connected to ground     |        |      |             |       |       |           |          |            |
|  32 | EXT_RSTB    | I    | External reset signal (Low: Active)                |        |      |             |       |       |           |          |            |
|  33 | GND         | P    | Ground                                             |        |      |             |       |       |           |          |            |


* UART0_TXD / UART1_TXD during flash programming

Planning the LIOT-ESP8266
-------------------------

The first focus of the board planning is, of course, on the ESP8266EX.
Many of the pins have a fixed purpose like the power supply, connections to an antenna or a crystal oscillator etc.
Admittedly, also the power supply offers a wide range of options, but nonetheless, the assignments of the GPIO's should be done first.

As already mentioned, the ESP8266EX has 16 GPIO's that are multiplexed with other functions.
For this reason, some of those have already a fixed purpose.

GPIO 3 will always be needed for UART receive and GPIO 1 is used for UART transmit while in normal operation.
When the chip is in programming mode, GPIO 2 serves as the UART transmit pin, but this will not be needed and thus GPIO 2 will continue to be used for other tasks available.
So, the single purpose of GPIO's 1 and 3 will be the connection to the USB-to-serial converter CP2102.

The external flash memory W25Q32FV respectively W25Q128FV is connected to the GPIO's from 6 to 11 which serve as SPI clock, MISO, MOSI, hold, write protect and chip select 0 (in this order).
Hold and write protect are misused to increase the write speed by providing the ``Quad IO'' mode.
The other two chip select pins should not be used for connecting other SPI slaves to avoid a misbehavior with the flash.

The other SPI interface - HSPI - is provided by the GPIO's from 12 to 15 which serve as MISO, MOSI, clock and chip select (in this order again).
Regrettably, there are no other chip select pins for HSPI so that only one devices can be connected, which will be the sub-1GHz RF transceiver CC1101.
In order to avoid interference and to be able to communicate with the transceiver durably, these GPIO pins should not connected differently.
Moreover, the use of a pull down resistor on chip select is recommended to avoid floating states.
This can be done as there is only one slave device.

Finally, GPIO 16 must be connected to the EXT\_RSTB pin of the ESP8266EX to be able to wakeup from deep sleep again.

So, only the four GPIO's 0, 2, 4 and 5 remain for free decisions.
Two pins will be needed for the use of software-I2C and thus connect the I2C slaves.
Furthermore, a possibility to receive interrupts from the slave devices port expander, nine-axis sensor and environmental sensor is needed as well as the possibility to reset the port expander.
The signaler needs a PWM signal and for all three LED's it would also be nice to be able to use PWM.
This would normally require ~10 more GPIO pins.

The requirement for dimmable LED's may be waived foremost.
Instead, they will be connected to the port expander to simply switch them on and off.
Pins 4 and 5 will be used for software-I2C.
The two remaining GPIO pins will be needed to reset the port expander and receive interrupts from it.
The port expander will always fire interrupts if the state of an input changes.
This eliminates the need for further interrupt enabled GPIO's on the ESP8266EX.

In order to get in the signaler, a trick will be used.
The signaler will be connected to the I2C clock line via a transistor.
Since the I2C interface will be implemented only by software, it can be achieved to obtain mutual exclusion of both components;
the signaler will only be enabled if the transistor is switched on by a GPIO of the port expander and thus will not make any noise if I2C is used, and the I2C slaves will not receive any data when the signaler is used and thus ignore the false "clock signal".

It doesn't really matter what the LIOT-ESP8266 board can do.
Rather, it is relevant what concepts are covered by the selected components.
These concepts include electrotechnical basics, connection interfaces, transmission standards and other features supported by the used hardware.

Interfaces: USART, SPI, I2C, I2S

RF: Sub-1-Ghz (Sigfox), Wi-Fi, Bluetooth, GSM

GPIO, Timers, RTC, PWM, ADC, DAC, SDIO, ...
 
 

| Pin | Name        | Type | GPIO       | SPI       | HSPI      | UART       | I2S      | Misc         | W25Q128   | CP2102    | CC1101   | PCA6416A   | I2C Bus     | Piezo | RGB-LED |
| ---:| ----------- | ---- | ---------- | --------- | --------- | ---------- | -------- | ------------ | --------- | --------- | -------- | --------   | ----------- | ----- |         |
|   8 | XPD_DCDC    | I/O  | **GPIO16** |           |           |            |          | **EXT_RSTB** |           |           |          |            |             |       |         |
|   9 | MTMS        | I/O  | GPIO14     |           | **CLK**   | U0_DSR     | I2SI_WS  |              |           |           | **SCLK** |            |             |       |         |
|  10 | MTDI        | I/O  | GPIO12     |           | **MISO**  | U0_DTR     | I2SI_SD  |              |           |           | **SDO**  |            |             |       |         |
|  12 | MTCK        | I/O  | GPIO13     |           | **MOSI**  | U0_CTS     | I2SI_SCK |              |           |           | **SDI**  |            |             |       |         |
|  13 | MTDO        | I/O  | GPIO15     |           | **/CS**   | U0_RTS     | I2SO_SCK |              |           |           | **nSEL** |            |             |       |         |
|  14 | GPIO2       | I/O  | **GPIO2**  |           |           | **²**      | I2SO_WS  |              |           |           |          | **SDI**    |             |       |         |
|  15 | GPIO0       | I/O  | **GPIO0**  | /CS2      |           |            |          |              |           | **/RTS²** |          | **/RESET** |             |       |         |
|  16 | GPIO4       | I/O  | **GPIO4**  |           |           |            |          | CLK_XTAL     |           |           |          | **SCK**    | **SCK**     |       |         |
|  18 | SDIO_DATA_2 | I/O  | GPIO9      | **/HOLD** | /HOLD     |            |          |              | **/HOLD** |           |          |            |             |       |         |
|  19 | SDIO_DATA_3 | I/O  | GPIO10     | **/WP**   | /WP       |            |          |              | **/WP**   |           |          |            |             |       |         |
|  20 | SDIO_CMD    | I/O  | GPIO11     | **/CS0**  |           | U1_RTS     |          |              | **/CS**   |           |          |            |             |       |         |
|  21 | SDIO_CLK    | I/O  | GPIO6      | **CLK**   |           | U1_CTS     |          |              | **CLK**   |           |          |            |             |       |         |
|  22 | SDIO_DATA_0 | I/O  | GPIO7      | **MISO**  |           | U1_TXD     |          |              | **DO**    |           |          |            |             |       |         |
|  23 | SDIO_DATA_1 | I/O  | GPIO8      | **MOSI**  |           | U1_RXD     |          |              | **DI**    |           |          |            |             |       |         |
|  24 | GPIO5       | I/O  | **GPIO5**  |           |           |            |          | CLK_RTC      |           |           |          | **/INT**   | **SDI**     |       |         |
|  25 | U0RXD       | I/O  | GPIO3      |           |           | **U0_RXD** | I2SO_SD  | CLK_XTAL     |           | **TXD**   |          |            |             |       |         |
|  26 | U0TXD       | I/O  | GPIO1      | /CS1      |           | **U0_TXD** |          | CLK_RTC      |           | **RXD**   |          |            |             |       |         |

<small>1. I2C slaves are PCA6416A (Port Expander, with extra column), BME280 (Temp / Hum / Baro), BMX055 (Accel / Gyro / Mag), APDS-9930 (Prox / Bright)</small>  
<small>2. during flash programming U0_TXD / U1_TXD, 3. during flash programming</small>

GPIO16 needs to be connected to EXT_RSTB if deep sleep wakeup should be possible

Missing Pins
------------

Si4463: nIRQ

PCA6416A: SCK, SDI, Address-Pins: ADDR(GND = 0x40/0x41, VDD = 0x42/0x43)

BME280: SCK, SDI, Address-Pins: SDO (GND = 0x76, VDD = 0x77)

BMX055: SCx, SDx, Address-Pins: SDO1, SDO2, CSB3 (0x10-0x13, 0x18, 0x19, 0x68, 0x69)

APDS-9930: SCL, SDA, INT

abmt-801 piezo


Enery Saving Options
--------------------

https://github.com/z2amiller/sensorboard/blob/master/PowerSaving.md


No free Pins left
-----------------

PWM?

Output 1 at port expander, AND with clock signal as PWM


Examples
--------

https://github.com/knewron-technologies/1btn

https://github.com/nkolban/esplibs

RTC https://blog.the-jedi.co.uk/2016/01/09/led-matrix-alarm-clock-update/

References
----------

http://ip.cadence.com/news/243/330/Tensilica-Unveils-Diamond-Standard-106Micro-Processor-Smallest-Licensable-32-bit-Core